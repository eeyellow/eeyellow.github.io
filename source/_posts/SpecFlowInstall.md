---
title: Use SpecFlow for BDD (1)
date: 2024-09-25 14:05:00
tags: [BDD, Test, SpecFlow]
categories: 
    - 程式
---

# **安裝SpecFlow**

SpecFlow官網已經有開新測試專案的[教學](https://docs.specflow.org/projects/getting-started/en/latest/)，這邊不贅述。

這邊紀錄把SpecFlow整合進現有的測試專案之步驟。

## 1. 安裝VisualStudio擴充功能

[SpecFlow for Visual Studio 2022](https://marketplace.visualstudio.com/items?itemName=TechTalkSpecFlowTeam.SpecFlowForVisualStudio2022&ssr=false#overview)

## 2. 安裝套件

在現有的 `xUnit` 測試專案中，安裝`SpecFlow.xUnit`

```bash
dotnet add package SpecFlow.xUnit
```

加入 `LivingDoc` 支援

```bash
dotnet add package SpecFlow.Plus.LivingDocPlugin
```

## 3. 加入Feature

建立資料夾結構：

```
BDD
├───Features
└───StepDefinitions
```

在`Features`資料夾上按右鍵 > 加入 > 新增項目 > `Feature File for SpecFlow`，檔名先用預設 `Feature1.feature`，之後可以再改。

預設的內容是這樣：

```Cucumber
Feature: Feature1

A short summary of the feature

@tag1
Scenario: [scenario name]
    Given [context]
    When [action]
    Then [outcome]
```

把它簡單修改成如下：

```Cucumber
@Home
@Login
Feature: 登入功能

使用者輸入帳密後送出
能夠得到登入的結果

#===========================================

Background:
    Given 帳號列表如下:
        | Account | Password |
        | user1   | pwd1     |
        | user2   | pwd2     |
        | user3   | pwd3     |

#===========================================

Scenario: 輸入正確的帳密，可以成功登入
    Given 輸入帳號為 "<account>"
    And 輸入密碼為 "<password>"
    When 按下登入按鈕
    Then 得到登入結果應為 "<isValid>"

Examples:
    | account | password | isValid |
    | user1   | pwd1     | true    |
    | user2   | pwd2     | true    |
    | user3   | pwd3     | true    |

#===========================================
```

建置專案，會發現自動產生出了 `Feature1.feature.cs`

## 4. 加入StepDefinition

上個步驟完成後，`Feature` 檔會出現顏色區別，其中 `Given-When-Then` 是紫色，這表示還沒有對應的 `StepDefinition`。

{% asset_img 01.png %}

按右鍵 > Define Steps > Create 

{% asset_img 02.png %}

把產生出的檔案，移到StepDefinitions資料夾中。

回到 `Feature` 檔，會發現 `Given-When-Then` 變成白色了，這表示已經有對應的 `StepDefinition`，並且可以按右鍵 > 移到定義，直接移到對應的程式碼。

{% asset_img 03.png %}

## 5. 修改StepDefinition

產生出來的 `StepDefinition` 預設類似這樣：

```csharp
[Binding]
public class 登入功能
{
    [Given(@"帳號列表如下:")]
    public void Given帳號列表如下(Table table)
    {
        throw new PendingStepException();
    }

    [Given(@"輸入帳號為 ""([^""]*)""")]
    public void Given輸入帳號為(string p0)
    {
        throw new PendingStepException();
    }

    [Given(@"輸入密碼為 ""([^""]*)""")]
    public void Given輸入密碼為(string p0)
    {
        throw new PendingStepException();
    }

    [When(@"按下登入按鈕")]
    public void When按下登入按鈕()
    {
        throw new PendingStepException();
    }

    [Then(@"得到登入結果應為 ""([^""]*)""")]
    public void Then得到登入結果應為(string @true)
    {
        throw new PendingStepException();
    }
}
```

需要實作它，以下是一個很簡單的實現

```csharp
[Binding]
public class 登入功能
{
    private readonly ScenarioContext _scenarioContext;
    private Dictionary<string, string> _mockUserTable = new Dictionary<string, string>();
    private string _account;
    private string _password;
    private bool _isValid;
    /// <summary>建構式</summary>
    public 登入功能(ScenarioContext scenarioContext)
    {
        _scenarioContext = scenarioContext;
    }

    [Given(@"帳號列表如下:")]
    public void Given帳號列表如下(Table table)
    {
        foreach(var row in table.Rows)
        {
            _mockUserTable.Add(row["Account"], row["Password"]);
        }
    }

    [Given(@"輸入帳號為 ""([^""]*)""")]
    public void Given輸入帳號為(string account)
    {
        _account = account;
    }

    [Given(@"輸入密碼為 ""([^""]*)""")]
    public void Given輸入密碼為(string password)
    {
        _password = password;
    }

    [When(@"按下登入按鈕")]
    public void When按下登入按鈕()
    {
        _isValid = _mockUserTable.Any(a => a.Key == _account
                                           && a.Value == _password);
    }

    [Then(@"得到登入結果應為 ""([^""]*)""")]
    public void Then得到登入結果應為(bool expectedResult)
    {
        Assert.Equal(_isValid, expectedResult);
    }
}
```
## 6. 加入更多 `Scenario`

```Cucumber
#===========================================

Scenario: 輸入的錯誤帳密，登入失敗
	Given 輸入帳號為 "<account>"
	And 輸入密碼為 "<password>"
	When 按下登入按鈕
	Then 得到登入結果應為 "<isValid>"

Examples:
	| account | password | isValid |
	| user1   | pwd123   | false   |
	| pwd2    | user2    | false   |
	| user3   | pwd3     | false   |

#===========================================
```

可以再依功能需求添加更多 `Scenario`

## 7. 執行測試

在測試總管視窗，可以看到所有的測試了，執行後會顯示結果。

{% asset_img 04.png %}

{% asset_img 05.png %}

## 8. 產生 `LivingDoc`

需要先安裝 CLI 工具

```bash
dotnet tool install --global SpecFlow.Plus.LivingDoc.CLI
```

然後到測試專案的 `bin\Debug\net8.0` (實際路徑依開發環境為準)，執行

```bash
# SpecFlowTestProject.dll 應替換為 [你的專案名稱].dll
livingdoc test-assembly SpecFlowTestProject.dll -t TestExecution.json
```

這樣可以產出 `LivingDoc.html`，直接打開就能看到測試規格與結果了。

{% asset_img 06.png %}

[官網教學](https://docs.specflow.org/projects/specflow-livingdoc/en/latest/LivingDocGenerator/Generating-Documentation.html)

## 9. 與Azure DevOps 管線整合

Azure DevOps 需要先安裝 [SpecFlow+LivingDoc Extension](https://marketplace.visualstudio.com/items?itemName=techtalk.techtalk-specflow-plus)

管線設定要先有一個 Task: dotnet test，後面再加上Task: SpecFlow LivingDoc

{% asset_img 07.png %}

```yaml
steps:
- task: techtalk.techtalk-specflow-plus.specflow-plus.SpecFlowPlus@0
  displayName: 'SpecFlow LivingDoc'
  inputs:
    generatorSource: TestAssembly
    testAssemblyFilePath: 'SpecFlowTestProject\bin\Release\net8.0\SpecFlowTestProject.dll'
    testExecutionJson: 'SpecFlowTestProject\bin\Release\net8.0\TestExecution.json'
    projectLanguage: en
    bindingAssemblies: 'SpecFlowTestProject\bin\Release\net8.0\SpecFlowTestProject.dll'
```

{% asset_img 08.png %}

這邊的設定僅供參考，需要依實際狀況做調整。

成功後就能把產生 `LivingDoc` 的步驟整合到CI裡面了

{% asset_img 09.png %}