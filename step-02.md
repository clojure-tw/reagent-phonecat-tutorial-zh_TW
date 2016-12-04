# Step-02：將資料以元件表示

**[Browse code](https://github.com/vvvvalvalval/reagent-phonecat-tutorial/blob/step-2/src/cljs/reagent_phonecat_tutorial/core.cljs#L1) - [Diff](https://github.com/vvvvalvalval/reagent-phonecat-tutorial/compare/step-1...step-2#diff-41da83b462fdcfe72936a8462288a3a7) - [Live demo](http://reagent-phonecat-tutorial.s3-website-us-east-1.amazonaws.com/step-02/)**

開始之前請執行 `git checkout step-2` 來切換分支到 step-2

***

現在我們知道如何在 Reagent 中建立視圖（views）並從資料動態產生資料的視圖。

### 添加模型（model）資料

首先，我們將手機列表加入到我們的命名空間（namespace）中。為簡單起見，我們會先直接建立手機列表，可想而知，也可以從網路或資料庫中來取得這些資料。

```clojure
(def hardcoded-phones-data [{:name "Nexus S" 
                             :description "Fast just got faster with Nexus S"}
                            {:name "Motorola XOOM™ with Wi-Fi" 
                             :description "The Next, Next Generation tablet."}])
```

### 以元件（component）定義我們的視圖

我們在視圖中定義 「*元件*」 來表示我們的資料，在 Reagent 中元件經常被定義成一個 ClojureScript 中簡單的函數，而這個函數是以資料作為輸入並回傳資料的 DOM 表示，就像是我們在前一個步驟中所看到的。

```clojure
(declare ;; here we declare our components to define they're in an order that feels natural.  
  <phones-list>
    <phone-item>)

(defn <phones-list> "An unordered list of phones"
  [phones-list]
  [:div.container-fluid
   [:ul
    (for [phone phones-list]
      [<phone-item> phone]
      )]])

(defn <phone-item> "An phone item component"
  [{:keys [name description] :as phone}]
  [:li.phone-item 
   [:span name]
   [:p description]])

(defn mount-root "Creates the application view and injects ('mounts') it into the root element." 
  []
  (rg/render 
    [<phones-list> hardcoded-phones-data]
    (.getElementById js/document "app")))

```

對於前一節的視圖而言，這是顯而易見的重構方式，這種定義讓程式碼顯得更具有彈性，又不會有更多的重複部分。

註： 對 Reagent 元件進行命名時，為了方便起見使用 `<component-name>` 的命名方式，這可能會讓人聯想到 HTML 標籤，這只是一個命名的便利方式，並且這也不會被 Reagent 做任何特殊的解釋（interpret）

然而當我們在撰寫元件函數時，我們不會用我們習慣的方式調用。舉例來說，在 `<phone-list>` 這個元件中，我們不會用例如像 `(<phone-item> phone)` 這樣的調用方式，我們只會將函數和函數的輸入放在一個向量中 `[<phone-item> phone]` 再交由 Reagent 去做特定的處理。

為什麼要這樣呢？因為這樣能讓 Reagent 「察覺」到元件間的層次結構，這使得 Reagent 能轉換我們定義的元件變成 React 的元件。

### 最後的優化

如果你用前面的程式碼重新開啟網頁，你會看到網頁顯示的都十分正常，不過會得到兩個警告在瀏覽器 console（browser console）：

```
    Warning: Every element in a seq should have a unique :key (in reagent_phonecat$core$phones_list). Value: [...]
```
與
``` 
    Warning: Each child in an array or iterator should have a unique "key" prop. Check the render method of    reagent_phonecat$core$phones_list. See https://fb.me/react-warning-keys for more information.
```

第二個警告是 React 關於列表中子元件的低級別警告，而第一個警告是 Reagent 告訴你如何來避免第二個警告

這是在 React 中你給予元件序列時會遇到的常見問題，React 警告你沒有在列表中提供對子元件的唯一識別，這會使得你在手機列表改變之後很難持續追蹤列表中的手機項目，你可以閱讀 [React documentation about dynamic children](http://facebook.github.io/react/docs/multiple-components.html#dynamic-children) 了解更多內容，要解決這個問題你得對每一個列表中的手機項目添加 `key` 屬性，我們可以透過在項目的元資料中定義 `:key` 值來解決這個問題：

```clojure
(defn <phones-list> "An unordered list of phones" 
  [phones-list]
  [:ul 
   (for [phone phones-list]
     ^{:key (:name phone)} [<phone-item> phone]
     )])
```

## 本章小結
## Summary

* 非常直觀的可以明白，一個 Reagent 元件是一個接受資料並回傳一個DOM 資料結構表示的函數，我們稱這個叫做 _rendering function_
* 在這個資料結構中，其他元件可能會被調用
* 在轉換時 Reagent 會轉換資料結構變成 React 的虛擬DOM
* 當你要一連串的元件序列在 rendering function 函數中，不要忘記加上元資料 `:key`

在**[step-03](https://github.com/vvvvalvalval/reagent-phonecat-tutorial/wiki/Step-03:-user-input-and-state)**中，我們會學習到如何處理使用者給予的輸入與狀態。
