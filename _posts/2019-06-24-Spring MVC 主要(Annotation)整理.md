---
layout: post
title:  "Spring MVC 主要(Annotation)整理"
date:   2019-06-24 01:20:28 +0900
categories: [Spring, Japanese]
---

```java
@Controller
```
スプリングMVCのコントローラオブジェクトであることを明示するエノテイション	(クラス)

&nbsp;

```spring
@RequestMapping(value = "/getList", method = {RequestMethod.POST})
// @PostMapping("/getList")に単純化できます。　
```
特定URIにマッチングされたクラスやメソッドであることを明示するエノテイション	(クラス、メソッド)       　　　　

&nbsp;

```spring
@RequestParam	// 要請(request)で特定のパラメータ値を見出すときに使用するエノテイション	パラメータ

private ModelAndView request_TEST(@RequestParam("test") int num,
@RequestParam("test2") String str)){
        // 上記のように一つ以上のタイプが適用できます。 スプリングからサポートする変換器からサポートされるすべてのタイプを変換可能です。
        // RequestParamは一つ以上パラメータにおいて使用可能です。
    }
```

@RequestHeader	要請(request)で特定HTTPヘッダ情報を抽出する際に使用	パラメータ
@PathVariable	現在のURIで希望する情報を抽出する際に使うエノテイション	パラメータ
@CookieValue	現在のユーザーのクッキーが存在する場合、クッキーの名前を利用してクッキーの値を抽出	パラメータ
@ModelAttribute	自動的に当該オブジェクトをビューまで伝えるように作るエノテイション	メソッド、パラメータ
@SessionAttribute	セッション上でモデルの情報を維持したい場合に使用 クラス
@InitBinder	パラメータを収集してオブジェクトを作る場合にカスタムイージング	メソッド
@ResponseBody	リターンタイプHTTPの応答メッセージで伝送	メソッド、リターンタイプ
@RequestBody	要請(request)文字列がそのままパラメータに渡し	パラメータ
@Repository	DAOオブジェクト	クラス
@Service	サービスオブジェクト	クラス

Reference:    
https://blog.hanumoka.net/2018/07/20/spring-20180720-Spring-MVC-Annotation/
https://0penster.tistory.com/24
