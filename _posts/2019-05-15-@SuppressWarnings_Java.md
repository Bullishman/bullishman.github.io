---
layout: post
title:  "@SuppressWarnings_Java"
date:   2019-05-15 00:21:28 +0900
categories: [Java, Japanese]
---

@SuppressWarningsを使用して警告を除く
Java 5.0からjava.lang.SuppressWarning アノテーションを使用してコンパイル単位のサブセットに関連するコンパイル警告を使用しないように設定することができます。         

```java
// Example
@SuppressWarning("unused")
public void foo() {
  Strings;
}
```

アノテーションがなければコンパイラでローカル変数s が使用できません。 アノテーションを使ってコンパイラはこの警告をfoo メソッドに対してローカルで無視します。 このような場合,同一のコンパイル単位または同一のプロジェクトの異なる位置に警告を保管することができます。       

SuppressWarningsアノテーション内部で使用できるトークンリストは以下の通りです。     

  - all すべての警告を抑えます。      
  - boxing boxing/unboxingオペレーションに関連する警告を抑えます。     
  - cast キャストオペレーションに関連する警告を抑えます。    
  - dep-ann 推奨されないアノテーションに関連する警告を抑えます。       
  - deprecation 推奨されない機能に関連する警告を抑えます。        
  - Fallthrough-switch 文から漏れたbreak 文に関する警告を抑えます。       
  - Finally リターンされない最後のブロックに関連する警告を抑えます。       
  - hiding 変数を隠すローカルに関連する警告を抑えます。        
  - incomplete-switch switch 文から漏れた項目と関連した警告を抑えます(enum case)。         
  - javadoc javadoc 警告に関連する警告を抑えます。        
  - nls ビnls 文字列リテラルに関連する警告を抑えます。      
  - null 君を(null)分析と関連した警告を抑えます。        
  - rawtypes 原始タイプの使い方に関する警告を抑えます。        
  - resource 閉じる可能性のあるタイプの資源使用に関する警告の抑制          
  - restriction を間違えたり,禁止された参照の使い方に関する警告を抑えます。        
  - serial 直列化可能クラスに対する抜けたserialVersionUIDフィールドに関連する警告を抑えます。         
  - ストレコ-access 間違った静的アクセスと関連した警告を抑えます。         
  - static-method staticで宣言されるようなメソッドに関連する警告を抑えます。         
  - Super スーパーの呼び出しを使用しないメソッドが重なり,使用に係る警告を抑えます。           
  - synthetic-access 内部クラスからの最適化されていないアクセスに係る警告を抑えます。             
  - sync-override 同期化されたメソッドをオーバーライドする場合,抜けた同期化による警告抑制             
  - unchecked 未確認オペレーションに関連する警告を抑えます。            
  - unqualified-field-access 規定されていないフィールドアクセスに関連する警告を抑えます。           
  - unused 使用していないコードおよび不必要なコードに関連する警告を抑えます。          

Reference: IBM Knowledge Center
