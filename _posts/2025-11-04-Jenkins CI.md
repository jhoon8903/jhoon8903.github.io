---
layout: post
title: 
subtitle: 
author: Daniel
categories: 
tags: 
banner:
  image:
---

## 1. Jenkins Job 구성
#### **Pre Step (락파일 정리 및 Unity 프로세스 종료)**
```bash
set -euo pipefail
pkill -f "/Contents/MacOS/Unity" || true
rm -f "$WORKSPACE/Temp/UnityLockfile" || true
rm -f "$WORKSPACE/Library/EditorInstance.json" || true
```

> Unity 가 열려 있으면 배치모드 실행이 막힌다.
> (로그에 “Another Unity instance is running…” 표시) → 필수 전처리.


#### **Invoke Unity 3D Editor**
```
-batchmode 
-quit 
-nographics 
-logFile "$WORKSPACE/Editor.log" 
-projectPath "$WORKSPACE" 
-executeMethod Project.EditorTools.BuildProcess.CI
-buildKind Both
-development false
-clean true
-out "Builds"
-branch "TestPlay"
```

- -executeMethod Project.EditorTools.BuildProcess.CI
    👉 namespace Project.EditorTools 안의 정적 메서드 CI() 를 실행한다.
    (namespace Editor 는 Unity 내부 UnityEditor.Editor 와 충돌하므로 변경 필수)

## 2. Build Process
경로: Assets/Editor/BuildProcess.cs

```csharp
#if UNITY_EDITOR  
using System;  
using System.IO;  
using System.Linq;  
using UnityEditor;  
using UnityEditor.Build.Reporting;  
using UnityEngine;  
  
namespace Project.EditorTools  
{  
    public static class BuildProcess  
    {  
        private static string Branch = "TestPlay";  
        private static string ProductName = Application.productName;  
        private static string Version = PlayerSettings.bundleVersion;  
        private static string OutputRoot = "Builds";  
        private static bool   Development = false;  
        private static bool   CleanOutput = true;  
        private static string WinExeName = "Editor";  
        private static string MacAppName = "Editor";  
          
        public enum BuildKind { Mac, Windows, Both }  
  
        public static void CI()  
        {  
            string[] args = Environment.GetCommandLineArgs();  
            BuildKind buildKind = ParseEnum(GetArg(args, key: "-buildKind"), def: BuildKind.Both);  
            ProductName = GetArg(args, key: "-productName", defaultValue: ProductName);  
            Version     = GetArg(args, key: "-version", defaultValue: Version);  
            OutputRoot  = GetArg(args, key: "-out", defaultValue: OutputRoot);  
            Development = ParseBool(GetArg(args, key: "-development"), def: Development);  
            CleanOutput = ParseBool(GetArg(args, key: "-clean"),       def: CleanOutput);  
            WinExeName  = GetArg(args, "-winExe", defaultValue: WinExeName);  
            MacAppName  = GetArg(args, "-macApp", defaultValue: MacAppName);  
            Branch = GetArg(args, "-branch", defaultValue: Branch);  
            if (!string.IsNullOrEmpty(ProductName)) PlayerSettings.productName = ProductName;  
            if (!string.IsNullOrEmpty(Version)) PlayerSettings.bundleVersion = Version;  
              
            bool success = true;  
            switch (buildKind)  
            {  
                case BuildKind.Mac:   
                    success &= BuildMac();    
                    break;  
                case BuildKind.Windows:   
                    success &= BuildWindows();   
                    break;  
                case BuildKind.Both:   
                    success &= BuildMac();   
                    success &= BuildWindows();   
                    break;  
            }  
  
            if (!success)  
            {  
                Debug.LogError("CI Build FAILED");  
                EditorApplication.Exit(1);  
            }  
            else  
            {  
                Debug.Log("CI Build SUCCESS");  
                EditorApplication.Exit(0);  
            }  
        }  
  
        #region Local Build Item  
  
        [MenuItem("Build/Run CI (Both)")]  
        public static void Menu_CI_Both()  
        {  
            if (!EditorUtility.DisplayDialog("Build", "Build Both (Mac + Windows)?", "Build", "Cancel")) return;  
            Development = false;  
            CleanOutput = true;  
            bool ok = BuildMac() & BuildWindows();  
            EditorUtility.DisplayDialog("Build", ok ? "SUCCESS" : "FAILED", "OK");  
        }  
  
        [MenuItem("Build/MacOS")]  
        public static void Menu_Mac() => BuildMac();  
  
        [MenuItem("Build/Windows")]  
        public static void Menu_Win() => BuildWindows();  
  
        #endregion  
        private static bool BuildMac()  
        {  
            string[] scenes = GetEnabledScenes();  
            string appName = string.IsNullOrEmpty(MacAppName) ? ProductName : MacAppName;  
            string outDir  = Path.Combine(OutputRoot, "mac");  
            string appPath = Path.Combine(outDir, $"{appName}.app");  
            if (CleanOutput && Directory.Exists(outDir)) Directory.Delete(outDir, true);  
            TrySetScriptingBackend(BuildTargetGroup.Standalone, ScriptingImplementation.IL2CPP);  
            BuildPlayerOptions options = new BuildPlayerOptions  
            {  
                scenes = scenes,  
                locationPathName = appPath,  
                target = BuildTarget.StandaloneOSX,  
                options = BuildOptionsFromFlags()  
            };  
            EditorUserBuildSettings.SwitchActiveBuildTarget(BuildTargetGroup.Standalone, BuildTarget.StandaloneOSX);  
            BuildReport report = BuildPipeline.BuildPlayer(options);  
            return LogReport("macOS", report);  
        }  
  
        private static bool BuildWindows()  
        {  
            string[] scenes = GetEnabledScenes();  
            string outDir = Path.Combine(OutputRoot, "win");  
            string exe = Path.Combine(outDir, $"{WinExeName}.exe");  
            if (CleanOutput && Directory.Exists(outDir)) Directory.Delete(outDir, true);  
            TrySetScriptingBackend(BuildTargetGroup.Standalone, ScriptingImplementation.Mono2x);  
            EditorUserBuildSettings.SwitchActiveBuildTarget(BuildTargetGroup.Standalone, BuildTarget.StandaloneWindows64);  
            BuildPlayerOptions options = new BuildPlayerOptions  
            {  
                scenes = scenes,  
                locationPathName = exe,  
                target = BuildTarget.StandaloneWindows64,  
                options = BuildOptionsFromFlags()  
            };  
            BuildReport report = BuildPipeline.BuildPlayer(options);  
            return LogReport("Windows", report);  
        }  
          
        private static string[] GetEnabledScenes() => EditorBuildSettings.scenes.Where(s => s.enabled).Select(s => s.path).ToArray();  
  
        private static BuildOptions BuildOptionsFromFlags()  
        {  
            BuildOptions opt = BuildOptions.None;  
            if (Development) opt |= BuildOptions.Development | BuildOptions.AllowDebugging | BuildOptions.ConnectWithProfiler;  
            return opt;  
        }  
  
        private static void TrySetScriptingBackend(BuildTargetGroup group, ScriptingImplementation impl)  
        {  
            try  
            {  
                PlayerSettings.SetScriptingBackend(group, impl);  
            }  
            catch (Exception e)  
            {  
                Debug.LogWarning($"SetScriptingBackend failed ({group}:{impl}): {e.Message}");  
            }  
        }  
  
        private static bool LogReport(string label, BuildReport report)  
        {  
            if (report == null)  
            {  
                Debug.LogError($"{label} build: report is null");  
                return false;  
            }  
  
            BuildSummary summary = report.summary;  
            string sizeMB  = (summary.totalSize / (1024f * 1024f)).ToString("F1");  
            switch (summary.result)  
            {  
                case BuildResult.Succeeded:  
                    Debug.Log($"✅ {label} build Succeeded | time: {summary.totalTime:mm\\:ss} | size: {sizeMB} MB\nOutput: {summary.outputPath}");  
                    return true;  
                case BuildResult.Failed:  
                    Debug.LogError($"❌ {label} build Failed | time: {summary.totalTime:mm\\:ss}");  
                    break;  
                case BuildResult.Cancelled:  
                    Debug.LogWarning($"⚠️ {label} build Cancelled");  
                    break;  
                case BuildResult.Unknown:  
                    Debug.LogWarning($"ℹ️ {label} build Unknown result");  
                    break;  
            }  
  
            for (int i = report.steps.Length - 1; i >= 0; i--)  
            {  
                BuildStep s = report.steps[i];  
                for (int index = s.messages.Length - 1; index >= 0; index--)  
                {  
                    BuildStepMessage m = s.messages[index];  
                    if (m.type is LogType.Error or LogType.Exception) Debug.LogError($"[{s.name}] {m.content}");  
                }  
            }  
  
            return false;  
        }  
          
        private static string GetArg(string[] args, string key, string defaultValue = null)  
        {  
            for (int i = 0; i < args.Length; i++)  
            {  
                if (args[i].Equals(key, StringComparison.OrdinalIgnoreCase) && i + 1 < args.Length) return args[i + 1];  
            }  
            return defaultValue;  
        }  
  
        private static bool ParseBool(string s, bool def)  
        {  
            if (string.IsNullOrEmpty(s)) return def;  
            if (bool.TryParse(s, out var b)) return b;  
            string t = s.Trim().ToLowerInvariant();  
            return t is "1" or "yes" or "y" or "true";  
        }  
  
        private static T ParseEnum<T>(string s, T def) where T : struct  
        {  
            if (string.IsNullOrEmpty(s)) return def;  
            return Enum.TryParse(s, true, out T v) ? v : def;  
        }  
          
        public static void Smoke()  
        {  
            string p = Path.Combine("Builds", "smoke.txt");  
            Directory.CreateDirectory("Builds");  
            File.WriteAllText(p, $"hello {DateTime.Now:O}");  
            Debug.Log($"[Smoke] wrote: {Path.GetFullPath(p)}");  
        }  
    }  
}  
#endif
```

## 3. Smoke Test
먼저 **Smoke()** 메서드로 CLI 호출 성공을 확인한다.

```csharp
public static void Smoke()  
{  
	string p = Path.Combine("Builds", "smoke.txt");  
	Directory.CreateDirectory("Builds");  
	File.WriteAllText(p, $"hello {DateTime.Now:O}");  
	Debug.Log($"[Smoke] wrote: {Path.GetFullPath(p)}");  
}  
```

> Editor command line arguments

```bash
-batchmode 
-quit 
-nographics 
-logFile "$WORKSPACE/Editor.log"
-projectPath "$WORKSPACE"
-executeMethod Project.EditorTools.BuildProcess.Smoke
```

로그에 [Smoke] wrote: 가 보이면 CLI 정상 작동.

## 4. 트러블
| **증상**                              | **원인**                    | **해결**                                              |
| ----------------------------------- | ------------------------- | --------------------------------------------------- |
| “No valid Unity license”            | 라이선스 미등록                  | Jenkins 환경변수 UNITY_LICENSE_CONTENT 사용 혹은 수동 ULF 등록  |
| “Another Unity instance is running” | 락파일 존재 또는 로컬 에디터 열림       | Pre Step 에서 Unity 종료 및 락파일 삭제                       |
| “executeMethod could not be found”  | 네임스페이스 오류 또는 Editor 외부 폴더 | namespace Project.EditorTools, Assets/Editor 경로로 이동 |
| “Native extension … not found”      | 빌드 모듈 미설치                 | mac-il2cpp, windows-mono 모듈 설치                      |
| Builds 폴더 없음                        | 출력 폴더 미생성 또는 씬 0개         | Directory.CreateDirectory() 및 씬 체크 추가               |
