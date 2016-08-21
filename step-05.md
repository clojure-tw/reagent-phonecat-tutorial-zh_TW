# Step 5：和伺服器進行溝通

**[Browse code](https://github.com/vvvvalvalval/reagent-phonecat-tutorial/blob/step-5/src/cljs/reagent_phonecat_tutorial/core.cljs#L1) - [Diff](https://github.com/vvvvalvalval/reagent-phonecat-tutorial/compare/step-4...step-5#diff-0fff143854a4f5c0469a3819b978a483) - [Live demo](http://reagent-phonecat-tutorial.s3-website-us-east-1.amazonaws.com/step-05/)**

開始之前請先執行 `git checkout step-5`。

可能需要重啓 Figwheel 服務程式並安裝需要的軟體。在終端機裡按下 ^C 停止 Figwheel 服務程式後，輸入：

```clojure
lein do deps, clean, figwheel
```

***

先前取巧把手機的資料寫死在程式裡，現在開始實際點，透過 HTTP 把將資料取回。

網頁伺服器在 `/phones/phones.json` 路徑下，提供了 JSON 格式的手機資料。要取得資料，下面是要做的事：

 * 發出 `GET /phones/phones.json` 的 AJAX HTTP 請求
 * 將 JSON 格式的回應轉換成 Clojure 的資料結構來使用
 * 更新應用程式的狀態以顯示取得的手機資料

一開始，atom 狀態是空的 vector

```clojure
(defonce state
  (rg/atom {:phones []
            :search ""
            :order-prop :name
            }))
```

Reagent 沒有 AJAX 相關的功能（好事一件，view library 不應在此着墨），因此這邊會使用 cljs-ajax。將它匯入後在我們的命名空間使用它。

**project.clj:**

```clojure
:dependencies [;; ...
                 [cljs-ajax "0.3.14"] 
                 ]
```

**core.cljs:**

```clojure
(ns reagent-phonecat.core
    (:require ;; ...
              [ajax.core :as ajx])
    )
```

寫個 `load-phones` 函式來取得手機的資料並更新應用程式狀態。

```clojure
(defn load-phones! "Fetches the list of phones from the server and updates the state atom with it" 
  [state]
  (ajx/GET "/phones/phones.json"
           {:handler (fn [phones] (swap! state assoc :phones phones))
            :error-handler (fn [details] (.warn js/console (str "Failed to refresh phones from server: " details)))
            :response-format :json, :keywords? true}))
```

拆解分析這個函式：

 - 將 atom 狀態當成參數傳入函式
 - `:handler (fn [phones] (swap! state assoc :phones phones))` 是會更新狀態的回呼函式
 - `:response-format :json, :keywords? true` 的意思是 "將回應資料以 JSON 表達，map 的鍵值是關鍵字而不是字串“
 - `:error-handler` 是出錯時會被呼叫的回呼函式，這裡的函式只記錄錯誤

最後，初始化時呼叫這些函式：

```clojure
(defn init! []
  (load-phones! state)
  (mount-root))
```

## 總結

學到使用 `cljs-ajax` 並實際用到 AJAX。

**[下一步](https://github.com/clojure-tw/reagent-phonecat-tutorial-zh_TW/blob/master/step-06.md)**.
