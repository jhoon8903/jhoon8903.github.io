---
layout: post
title: Object Pool을 이용한 장애물 컨트롤
subtitle: Unity Assemable 에서 제공하는 오브젝트 풀링
author: Daniel
categories: Unity
tags: 
 - Unity
 - Programming
banner:
  image: https://i.imgur.com/eSSL35S.jpg
---
![](https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F573d499f-80ac-4e49-a243-d5079503ca40%2F3.png?table=block&id=d5e15def-1ac2-420f-9c62-49b36a9a637e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2)

오브젝트풀링
--

- 유니티 어셈블에서 제공하는 풀링 시스템을 이용한 코드 리팩토링
- 기존 풀필 시스템은 풀링의 갯수를 정해두고 미리 오브젝트를 풀링 해둔 상태로, 리스트에 담아서 하나하나 꺼내 쓰는 방법이었다면, 새로운 풀링은 동적으로 필요에 따라 생성하고 추가적으로 더 필요하다면 런타임에서 풀을 생성하여 계속해서 재활용하는 방법입니다.


### 기존 풀링 시스템

```csharp
using System.Collections.Generic;  
using UnityEngine;  
using UnityEngine.SceneManagement;  
  
public class ObstaclePool : MonoBehaviour  
{  
    private List<GameObject> pooledObstacles;  
    public List<GameObject> obstaclePrefabs;  
    private int pooledAmount = 50;  
    
    void Start()  
    {        
	    InitializePool();  
    }  
    
    private void InitializePool()  
    {        
	    pooledObstacles = new List<GameObject>();  
  
        for (int i = 0; i < pooledAmount; i++)  
        {            
	        int prefabIndex = Random.Range(0, obstaclePrefabs.Count);  
            GameObject obstacle = Instantiate(obstaclePrefabs[prefabIndex]);  
            obstacle.name = obstaclePrefabs[prefabIndex].name;  
            obstacle.SetActive(false);  
            obstacle.transform.SetParent(transform);  
            obstacle.transform.position = new Vector2(obstacle.transform.position.x, obstacle.transform.position.y);  
            pooledObstacles.Add(obstacle);  
        }    
	}  
  
    public GameObject GetPooledObstacle(int prefabIndex)  
    {        
	    if (prefabIndex < 0 || prefabIndex >= obstaclePrefabs.Count)  
        {            
	        return null;  
        }  
        
        for (int i = 0; i < pooledObstacles.Count; i++)  
        {            
	        if (pooledObstacles[i] != null && !pooledObstacles[i].activeInHierarchy && pooledObstacles[i].name.Contains(obstaclePrefabs[prefabIndex].name))
            {                
	            return pooledObstacles[i];  
            }        
		}  
        GameObject newObj = Instantiate(obstaclePrefabs[prefabIndex]);  
        newObj.name = obstaclePrefabs[prefabIndex].name;  
        newObj.SetActive(false);  
        newObj.transform.position = new Vector2(newObj.transform.position.x, newObj.transform.position.y);  
        pooledObstacles.Add(newObj);  
        return newObj;  
    }  
    
    public void ReturnToPool(GameObject obstacle)  
    {        
	    if (obstacle != null)  
        {            
	        obstacle.SetActive(false);  
        }    
	}  
	
    public void ResetPool()  
    {        
	    foreach (GameObject obstacle in pooledObstacles)  
        {            
	        if (obstacle != null)  
            {                
	            Destroy(obstacle);  
            }        
		}  
        pooledObstacles.Clear();  
        InitializePool();  
    }
}
```

- 위의 방법은 장애물을 풀링할때 얼마나 쓰일지 모르는 오브젝트들을 일괄적으로 50개씩 만들어 두어 하나하나 사용하는 방법으로 불필요한 리소스 낭비가 발생을 하는 문제가 있습니다.

### 개선된 풀링 방식

```csharp
using Manager;  
using UnityEngine;  
using UnityEngine.Pool;  
  
namespace Objects  
{  
    public class Pool  
    {  
        private readonly GameObject _prefab;  
        private readonly IObjectPool<GameObject> _pool;  
        private Transform _root;  
  
        private Transform Root  
        {  
            get  
            {  
                if (_root != null) return _root;  
                
                GameObject obj = new()  
                {                    
	                name = $"[Pool_Root] {_prefab.name}"  
                };              
                Transform baseObstacleTransform = ServiceLocator.GetService<ObstacleManager>().BaseObstacle.transform;  
                obj.transform.SetParent(baseObstacleTransform, false);  
                _root = obj.transform;  
                return _root;  
            }        
		}  
        
        public Pool(GameObject prefab)  
        {            
	        _prefab = prefab;  
            _pool = new ObjectPool<GameObject>(OnCreate, OnGet, OnRelease, OnDestroy);  
        }
          
        public GameObject Pop()  
        {            
	        return _pool.Get();  
        }  
        
        public void Push(GameObject obj)  
        {            
	        if (obj == null) return;  
            _pool.Release(obj);  
        }  
        
        private GameObject OnCreate()  
        {            
	        GameObject obj = Object.Instantiate(_prefab, Root, true);  
            obj.name = _prefab.name;  
            return obj;  
        } 
         
        private void OnGet(GameObject obj)  
        {            
	        if (obj == null) return;  
            obj.SetActive(true);  
        }  
        
        private void OnDestroy(GameObject obj)  
        {            
	        Object.Destroy(obj);  
        }  
        private void OnRelease(GameObject obj)  
        {            
	        if (obj == null) return;  
            obj.SetActive(false);  
        }    
	}
}
```

- `using UnityEngine.Pool` 을 사용하여 풀 구성을 합니다.
- `private readonly IObjectPool<GameObject> _pool` 인터페이스로 정의된 메서드로 풀 시스템을 만듭니다.
#### IObjectPool

```csharp
namespace UnityEngine.Pool  
{  
  public interface IObjectPool<T> where T : class  
  {  
    int CountInactive { get; }  
  
    T Get();  
  
    PooledObject<T> Get(out T v);  
  
    void Release(T element);  
  
    void Clear();  
  }
}
```

#### ObjectPool

```csharp
namespace UnityEngine.Pool  
{  
  /// <summary>  
  ///   <para>A stack based Pool.IObjectPool_1.</para>  /// </summary>  public class ObjectPool<T> : IDisposable, IObjectPool<T> where T : class  
  {  
    internal readonly List<T> m_List;  
    private readonly Func<T> m_CreateFunc;  
    private readonly Action<T> m_ActionOnGet;  
    private readonly Action<T> m_ActionOnRelease;  
    private readonly Action<T> m_ActionOnDestroy;  
    private readonly int m_MaxSize;  
    internal bool m_CollectionCheck;  
  
    public int CountAll { get; private set; }  
  
    public int CountActive => this.CountAll - this.CountInactive;  
  
    public int CountInactive => this.m_List.Count;  
  
    public ObjectPool(  
      Func<T> createFunc,  
      Action<T> actionOnGet = null,  
      Action<T> actionOnRelease = null,  
      Action<T> actionOnDestroy = null,  
      bool collectionCheck = true,  
      int defaultCapacity = 10,  
      int maxSize = 10000)  
    {      
	    if (createFunc == null)  
        throw new ArgumentNullException(nameof (createFunc));  
      if (maxSize <= 0)  
        throw new ArgumentException("Max Size must be greater than 0", nameof (maxSize));  
      this.m_List = new List<T>(defaultCapacity);  
      this.m_CreateFunc = createFunc;  
      this.m_MaxSize = maxSize;  
      this.m_ActionOnGet = actionOnGet;  
      this.m_ActionOnRelease = actionOnRelease;  
      this.m_ActionOnDestroy = actionOnDestroy;  
      this.m_CollectionCheck = collectionCheck;  
    }  
    
    public T Get()  
    {      
	    T obj;  
      if (this.m_List.Count == 0)  
      {        
	      obj = this.m_CreateFunc();  
        ++this.CountAll;  
      }      
      else  
      {  
        int index = this.m_List.Count - 1;  
        obj = this.m_List[index];  
        this.m_List.RemoveAt(index);  
      }      
      
      Action<T> actionOnGet = this.m_ActionOnGet;  
      
      if (actionOnGet != null) actionOnGet(obj);      
      return obj;  
    }  
    
    public PooledObject<T> Get(out T v)  
    {      
	    return new PooledObject<T>(v = this.Get(), (IObjectPool<T>) this);  
    }  
    
    public void Release(T element)  
    {      
	    if (this.m_CollectionCheck && this.m_List.Count > 0)  
	    {        
	      for (int index = 0; index < this.m_List.Count; ++index)  
	      {          
		      if ((object) element == (object) this.m_List[index])  
            throw new InvalidOperationException("Trying to release an object that has already been released to the pool.");  
		  }      
	  }      
	  
	  Action<T> actionOnRelease = this.m_ActionOnRelease;  
      
      if (actionOnRelease != null) actionOnRelease(element);
      if (this.CountInactive < this.m_MaxSize)  
      {        
	      this.m_List.Add(element);  
      }      
      else  
      {  
        Action<T> actionOnDestroy = this.m_ActionOnDestroy;  
        
        if (actionOnDestroy != null) actionOnDestroy(element);
      }    
	}  
    
    public void Clear()  
    {      
	    if (this.m_ActionOnDestroy != null)  
	    {        
		    foreach (T obj in this.m_List)  
	          this.m_ActionOnDestroy(obj);  
        }      
        this.m_List.Clear();  
        this.CountAll = 0;  
    }  
    
    public void Dispose() => this.Clear();  
  }
}
```

- 미리 `UnityEngine.Pool` 네임스페이스에 선언된 Pool 시스템으로 가져와 사용합니다.

#### Get()

- 풀 리스트에 저장된 오브젝트를 불러옵니다.
- 이때 Pool 시스템의 Get을 통해 pool 리스트 내부의 오브젝트에 접근합니다.

#### Release

- 사용중이던 오브젝트를 비활성화 하여 다시 Get() 을 통해 사용가능하도록 준비합니다.

### PoolManager

```csharp
using System.Collections.Generic;  
using Objects;  
using UnityEngine;  
  
namespace Manager  
{  
    public class PoolManager  
    {  
        private readonly Dictionary<string, Pool> _pools = new();  
  
        public GameObject Pop(GameObject prefab)  
        {            
	        if (!_pools.ContainsKey(prefab.name))  
            {                
	            CreatePool(prefab);  
            }            
            return _pools[prefab.name].Pop();  
        }  
        
        public bool Push(GameObject obj)  
        {            
	        if (!_pools.ContainsKey(obj.name)) return false;  
            _pools[obj.name].Push(obj);  
            return true;  
        } 
         
        private void CreatePool(GameObject prefab)  
        {            
	        Pool pool = new(prefab);  
            _pools.Add(prefab.name, pool);  
        }    
	}
}
```

- 실제 스크립트 상에서 사용되는 Pool Manager 입니다.
- `Pop`, `Push`를 사용하여 오브젝트가 `_pools`에 존재하지 않는다면 추가로 생성하고 풀에 넣어 꺼내씁니다.
- Push 로는 사용한 오브젝트를 반환합니다.

### 리소스매니져와의 연결

```csharp
public GameObject InstantiateObject(string key, Transform parent = null, bool pooling = false)  
{  
    GameObject resource = Load<GameObject>($"{key}.prefab");  
    return pooling ? ServiceLocator.GetService<PoolManager>().Pop(resource) : Utility.InstantiateObject(resource, parent);  
}
```

- 리소스 매니져와 연결하여 인스턴스를 생성시에 pooling의 bool 여부에 따라서 인스턴스를 생성 또는 pool 매니저에 넘겨 처리 합니다.

### 오브젝트 반환

```csharp
protected virtual IEnumerator DeactivateAfterDelay()  
{  
    yield return new WaitForSeconds(Delay);  
    ServiceLocator.GetService<PoolManager>().Push(gameObject);  
}
```

- 사용을 다한 오브젝트는 Pool manager에서 Push로 해당 오브젝트를 반환합니다.

마치며
--

이미 내부적으로 구현이 완료가 되어있기 때문에 별도의 스크립작성 분량이 많지 않아서 빠르게 구현이 가능하며, 인터페이스로 구현이 되어 있어, 팀 협업시에도 풀링 작성시 통일된 스크립트를 제공할 수 있습니다.