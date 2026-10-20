---
title: "提升 AI Flutter 開發能力的 Dart and Flutter MCP server"
source: "https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-flutter-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E6%8F%90%E5%8D%87-ai-flutter-app-%E9%96%8B%E7%99%BC%E8%83%BD%E5%8A%9B%E7%9A%84-dart-and-flutter-mcp-server-305720171089"
author:
  - "[[彼得潘的 iOS App Neverland]]"
published: 2025-08-17
created: 2026-05-25
description: "透過官方推出的 Dart and Flutter MCP server，AI 可以更聰明地開發 Flutter App，幫忙我們解決各種問題，比方修正 runtime error，從 pub.dev 搜尋安裝適合的套件等。"
tags:
  - "clippings"
status: processed
---
透過官方推出的 Dart and Flutter MCP server，AI 可以更聰明地開發 Flutter App，幫忙我們解決各種問題，比方修正 runtime error，從 pub.dev 搜尋安裝適合的套件等。

以下我們先介紹如何在 IDE 啟用 Dart and Flutter MCP server，然後再示範它修正 runtime error 的強大能力。

## 在 IDE 啟用 Dart and Flutter MCP server

以搭配 GitHub Copilot 的 VS Code 為例，打開 Settings (UI) 畫面。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*66WMr4xIgFJCH-SdRMwnaQ.png)

勾選 Dart: Mcp Server。(ps: 記得 Dart 的版本必須 3.9 以上)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*lTELUDNosNY4-I2G4xgZ2w.png)

點選 GitHub Copilot 的 Configure Tools 按鈕。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ffeYY3c2ohzDep5Oyra2ow.png)

確認 MCP Server: Dart SDK MCP Server 被勾選。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*el5xchsNUhUofHGu05-hvA.png)

其它 IDE 的啟用方法請參考官方文件的說明。

## [Dart and Flutter MCP server](https://dart.dev/tools/mcp-server?source=post_page-----305720171089---------------------------------------)

### Learn about the Dart and Flutter MCP server tool that exposes Dart and Flutter tools to compatible AI-assistant clients…

dart.dev

## 修正 runtime error

以下我們故意讓 MyHomePage Row 裡文字太長塞不下。

```rb
class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Row(
          spacing: 10,
          children: [
            Container(
              color: Colors.lightBlue,
              child: Text('Peter Pan', style: TextStyle(fontSize: 30)),
            ),
            Container(
              color: Colors.lightGreen,
              child: Text('Dreams do come true, if only we wish hard enough.'),
            ),
          ],
        ),
      ),
    );
  }
}
```

App 執行後將出現黃黑條紋問題。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ZqxReNTjii_83SBMIxKK3Q.png)

Debug Console 顯示錯誤訊息 RenderFlex overflowed。

```rb
════════ Exception caught by rendering library ═════════════════════════════════
The following assertion was thrown during layout:
A RenderFlex overflowed by 96 pixels on the right.

The relevant error-causing widget was:
    Row Row:file:///Users/shih-yingpan/Desktop/flutter_application_9/lib/main.dart:45:16
```

現在讓我們試試 Dart and Flutter MCP server 的威力，請它修正錯誤吧。

## Get 彼得潘的 iOS App Neverland’s stories in your inbox

Join Medium for free to get updates from this writer.

在 GitHub Copilot 的輸入框輸入以下 prompt。

```rb
fix runtime error
```
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Y1FaK-hX68VE5F_MCFPlAQ.png)

AI 利用 Dart and Flutter MCP server 找到問題是 RenderFlex overflowed。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*26EcTBWVcNV5VAm4ooyTRA.png)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*b1tCvVa0FDHC0Dp7tykxUA.png)

AI 成功修正問題，加入了 Expanded widget。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*h9WdPpI-Wn6p6deV1YD9iA.png)

點選 Keep 採用 AI 修改的程式，點選 Continue 觸發 hot reload。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*-TgH423FHuekOG2-R_Zp8Q.png)

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*VCC-fnPtIWDZsTE5BC6Hpg.png)

App 畫面一切正常，再也沒有黃黑條紋了。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*3WybEafeWC7YCVQ30iKHUg.png)