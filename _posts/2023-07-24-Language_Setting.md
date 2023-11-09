---
layout: post
title: Language Setting
subtitle: 언어 모델 세팅
categories: Project
author: Daniel
tags: 
 - Unity
 - Project
 - Game
 - Script
 - Data
banner:
 image: https://i.imgur.com/Fynb3bN.png
---

## Language Setting 

언어는 기획에서 설정가능하도록 CSV화 하고 이를 불러오는 역할만 수행

```csharp
using System.Collections.Generic;
using UnityEngine;

namespace Script.RewardScript
{
    public class Language : MonoBehaviour
    {
        public enum LanguageType { Korean, English }
        public LanguageType selectedLanguage = LanguageType.Korean;
        private readonly Dictionary<string, Dictionary<string, string>> 
        _translations = new Dictionary<string, Dictionary<string, string>>();
        public void Awake()
        {
            if (PlayerPrefs.HasKey("Language"))
            {
                var savedLanguage = PlayerPrefs.GetString("Language");
                selectedLanguage = savedLanguage switch
                {
                    "KOR" => LanguageType.Korean,
                    "ENG" => LanguageType.English,
                    _ => selectedLanguage
                };
            }
            var csvFile = Resources.Load<TextAsset>("languageData");
            var csvData = csvFile.text.Split(new[] { "\r\n", "\r", "\n" }, 
            System.StringSplitOptions.None);
            for (var i = 1; i < csvData.Length; i++)
            {
                var data = csvData[i].Split(',');
                var type = data[0];
                var kor = data[1];
                var eng = data[2];

                if (!_translations.ContainsKey(type))
                {
                    _translations[type] = new Dictionary<string, string>();
                }
                _translations[type]["Kor"] = kor;
                _translations[type]["Eng"] = eng;
            }
        }

        public string GetTranslation(string type)
        {
            var selectedLang = selectedLanguage == LanguageType.Korean 
            ? "Kor" 
            : "Eng";
            if (!_translations.TryGetValue(type, out var typeTranslations)) 
            return string.Empty;
            return 
            typeTranslations.TryGetValue(selectedLang, out var translation) 
            ? translation 
            : string.Empty;
        }
    }
}
```

```csharp
 private void LevelUpDisplayText
 (Button expButton, TMP_Text powerText, TMP_Text powerCode, Image btnBadge, 
 ExpData powerUp, Language selectedLanguage)
        {
            var translationKey = powerUp.Type.ToString();
            var powerTextTranslation = 
            selectedLanguage.GetTranslation(translationKey);
            var p = powerUp.Property[0].ToString();
            var finalPowerText = powerTextTranslation.Replace("{p}", p);
            var placeholderValues = new Dictionary<string, Func<double>> {
                { "{15*EnforceManager.Instance.slowCount}", () => 15 * 
                EnforceManager.Instance.slowCount },
                .......

            };
            finalPowerText = 
            placeholderValues.Aggregate(finalPowerText, (current, placeholder) 
            => current.Replace(placeholder.Key, placeholder.Value().ToString()));
            var finalTranslation = finalPowerText.Replace("||", "\n");
            powerText.text = powerUp.Type switch
            {
                ExpData.Types.GroupDamage => finalTranslation,
                ExpData.Types.GroupAtkSpeed => finalTranslation,
                .......
            }
```