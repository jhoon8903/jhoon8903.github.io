---
title: Status Effect
category: unity
author: 이정훈
tags:
  - unity
  - c#
  - game
img: https://i.imgur.com/jah0mIQ.gif
comments_disable: true
meta_description: Status Effect
---

![](https://i.imgur.com/jah0mIQ.gif)
## Enemy Status Effect 

```csharp
// 상태이상로직
        public int groupSlowCount;
        public bool IsRestraint { get; set; } = false;
        public bool IsSlow { get; set; } = false;
        private Coroutine _poisonEffectCoroutine;
        private bool _isPoison;
        public bool IsPoison
        {
            get => _isPoison;
            set
            {
                _isPoison = value;
                if (_isPoison)
                {
                    var venomSac = FindObjectOfType<VenomSac>();
                    if (venomSac != null && gameObject.activeInHierarchy)
                    {
                        _poisonEffectCoroutine = 
                        StartCoroutine(venomSac.PoisonEffect(this));
                    }
                }
                else
                {
                    if (_poisonEffectCoroutine == null || !(healthPoint <= 0)) 
                    return;
                    StopCoroutine(_poisonEffectCoroutine);
                    gameObject.GetComponent<SpriteRenderer>().DOColor(new 
                    Color(1f, 1f, 1f), 0.2f);
                    _poisonEffectCoroutine = null;
                }
            }
        }
```

{get; set;} 을 이용한 상태 bool 값에 따라서 각족 Effect 적용이 가능
그리고 적용된 Effect는 중복 상태이상을 방지하고, 상태이상시 Color을 변경

