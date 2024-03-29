# Don't Make My Mistakes: Common Infrastructure Errors I've Made

Article - DevOps

Mathew Duggan

03 Dec 2021 • 9 min read

隨著我的職業生涯的發展，一種超現實的體驗是在會議期間你被強烈的似曾相識的感覺。 時不時，有人會提到一些事情，你會閃回到你關於這幾個的同一個會議 工作前。 然後做出了一個決定，一個糟糕的選擇毀了你幾個月的工作生活。
你回到現在，幾乎是從椅子上跳起來反對，“不要做 X！”。 你的同事被你的激烈反應，但他們沒有看到你的恐懼。

我想花點時間寫下我犯的一些最嚴重的錯誤，作為對以後可能出現的其他人的警告。 別擔心，你會自己犯所有的新錯誤。 但請允許我花點時間回顧一些最災難性的決定或我曾經同意過的項目（or even fought to do, sometimes）。

# 不要將應用程序從數據中心遷移到雲

啊，雲服務的警報聲。我是他們的忠實粉絲個人而言，但為 physical 數據中心 設計的應用程序 "很少" 能夠無縫遷移到雲中。 我一直 現在參與了三項嘗試，將為特定數據中心編寫的應用程序大規模遷移到 雲， 每次我都在對環境的
undocumented assumptions 的岩石上崩潰。 我遇到了第一個無法解決的數據中心到雲遷移問題

當開發人員編寫和測試應用程序時，他們對他們的環境將如何運作產生期望。 服務器如何工作，什麼樣的我的應用程序獲得的性能，網絡的可靠性，我可以期待什麼樣的延遲等等。 這些是 任何人在一個環境中工作多年都會做的合理的事情，但這意味著當你打包
啟動一個應用程序並在其他地方運行它，尤其是舊應用程序，奇怪的事情發生了。

## 你之前從未遇到過的錯誤 開始彈出，需要做出各種 奇怪的 架構決策 來嘗試並允許 這種轉變。

很快你就消除了遷移的很多價值，甚至可能做了一些可怕的事情， 比如 通過直接連接將您的數據中心連接到 AWS，以嘗試無縫橋接這兩個環境。

## 你的清單 複雜的決策開始增多，hitting increasingly more and more edge cases of your cloud provider。

不可避免地，您會發現一些您無法移動的東西，並且您現在被困在兩個環境中， 一個您需要移動的數據中心 維護和一個新的雲帳戶。你為你的傲慢感到惋惜。

相反....

## Port the application to the cloud.

給開發者一個完全與數據中心隔離的環境，讓他們將應用程序移植到雲端，然後為您的應用程序安排 4-8 小時的停機時間。 這將允許持久層進行切換，然後您可以更改您的 DNS 條目以指向您的新雲存在。
嘗試防止這種停機時間會使您陷入一個又一個錯誤決定的錯誤決定中。最好咬緊牙關繼續前進。

或者更好的是，在您希望運行它的相同環境中開發您的應用程序。 Or even better, develop your application in the same environment you expect to run it in.

# 不要寫你自己的秘密系統

我不知道為什麼我一直遇到這個。出於某種原因，組織喜歡 編寫自己的機密管理系統。通常這些是由基礎設施團隊編寫的應用程序，通常 環境變量注入系統或某種基於 RSA 密鑰的解密 API 調用。連我都墮落了
這個想法的受害者，認為“當然它不會那麼困難”。

出於某種原因，也許我失去了理智或什麼，我決定我們要在裡面管理我們的秘密 我將管理的 PostgREST 應用程序。我編寫了一個應用程序，它會生成 JWT 並將其返回給應用程序 取決於各種標準。這些將允許他們以完全安全的方式訪問他們的秘密。

現在為 PostgREST 辯護，它很好地完成了它所承諾的事情。但秘密管理的問題更多 比它最初出現的複雜。首先我們碰到了緩存的問題，你如何避免碰到這個服務 每小時百萬次，但仍然保持一些使用服務器作為事實來源的概念。這是可以解決的 通過一些
Nginx 配置，但這是我應該想到的。

然後我用旋轉的耙子打自己的臉。推送新版本很簡單，但秘密不是 通常版本化到客戶端。我對我的應用程序進行了身份驗證，我看到了正確的秘密。但是在輪換期間 期間有兩個正確的秘密，當我說出來時很明顯，但在我寫它時卻沒有想到。
同樣，這不是一件很難解決的事情，但是隨著時間的推移，我遇到了越來越多的服務邊緣情況，我 意識到我犯了一個巨大的錯誤。

現實情況是，機密管理是一種典型的高風險低迴報服務。它不會幫助我的客戶 直接，我運行它不會給領導層的任何人留下深刻印象，它會消耗我很多時間調試它並且 就運行它而言，它需要大量特定於領域的知識。我不得不重新思考很多作品，因為我
去了，從多區域可用性（比如跨區域同步是一種拖累）到強化服務。

相反......

只需使用 AWS Secrets Manager 或 Vault。我更喜歡 Secrets Manager，但無論您喜歡什麼都可以。只是不要 自己寫，有很多邊緣情況，但好處並不多。您將成為所有應用程序的原因
在一天結束時節省的成本是微乎其微的。

# 不要運行自己的 Kubernetes 集群

我知道，你有技術能力去做。也許你非常喜歡跑步 etcd 並設置各種證書。這是一個非常簡單的決策樹，用於考慮“我應該運行我的 是否擁有 k8s 集群”：

您是財富 100 強公司嗎？如果沒有，那就不要做。

原因是您不必這樣做，讓其他人運行它可以讓您充分利用這一切 他們添加的功能。 AWS EKS 具有一些令人難以置信的功能，從在您的 kubeconfig 文件中支持 AWS SSO 到 允許您在 ServiceAccounts 內使用 IAM
角色來訪問 AWS 資源的 pod。最重要的是，他們 將運行您的控制平面一年不到 1000 美元。暫時擱置這一切，讓我們坦率地談談 第二。

## 雲的優勢之一是其他人為您進行 Beta 測試升級。

我不明白為什麼人們不再談論這個。是的，您可以非常成功地運行自己的 k8s 集群，但是 為什麼？我實際上有成千上萬的 Beta 測試人員排在我前面，以確保 EKS 升級工作。在之上 那個，我有很多 AWS 工程師在做這件事。如果我要在 AWS
中運行我的基礎設施，沒有任何優勢 無論如何運行我自己的集群，除了我可以保持在某個時候我可以“切換雲 提供者”。這讓我進入下一點。

反而....

讓雲提供商運行它。現在是他們的問題。專注於讓您的開發人員的生活更輕鬆。

# 不要為多個雲提供商設計

這一點在個人層面上讓我很惱火。我被一個非常 有說服力的經理，我們需要確保我們有能力切換雲提供商。與我更好的判斷相反，我 誤入歧途。我們稱他們為“過早優化”人群。

很快，我開始審核“多雲兼容性”的新服務，確保而不是使用來自 AWS，我們自己維護。這將使我們能夠在不太可能發生的情況下在它們之間切換 公司人氣暴漲，我們大到足以從這次遷移中受益。我猜在我們的集體
認為這是某種未來的證明，或者也許我們只是對宏偉的妄想。

我們實際上在做的是你能做的最糟糕的事情，這對那些試圖 將功能交付給客戶。如果您在 AWS 中，請不要假裝您的應用程序確實需要 可部署到多個雲。如果 AWS 明天消失，是的，您將需要遷移您的應用程序。但該 AWS
比您的公司更長壽的可能性很高，並且維護您自己的云不可知的時間投資 翻譯層不是你可能會得到的。

我們最終得到了一堆從未更新過最新功能的庫，這意味著開發人員 不斷閱讀有關他們無法使用或嘗試的 AWS 一些很棒的新功能的信息。教程顯然沒有 與我們出色的自定義庫一起工作，我們從未最終切換雲提供商甚至雙重部署，因為
從經濟上講，這樣做從來沒有意義。我們最終只吃掉了整個開發團隊的一大堆烏鴉。

反而....

如果有人說“我們需要確保我們不依賴於一個雲提供商”，請告訴他們那艘船在您第二次航行 註冊。與數據中心類似，在 AWS 中設計、測試和成功運行多年的應用程序很可能 了解該環境的一些期望和模式。嘗試針對不可知設計進行優化正在失去很多
雲提供商的價值，並為您和其他所有人增加大量繁忙的工作。

不要成為那種人。沒有人喜歡在會議中不斷說“不，我們不能那樣做”的人。如果你發現 在遷移到新的提供商具有經濟意義的情況下，請留出至少 3 個月的時間 用於測試和移植的應用程序。看看在那之後它是否仍然具有財務意義。

雲提供商是一種依賴，就像編程語言一樣。不認真是不能隨意切換的 考慮，即使如此，“移植”通常也是錯誤的選擇。通常，您想像演奏一樣練習，所以 在與您的客戶將使用您的產品相同的環境中進行開發。

# 不要讓警報無限增長

我相信您在工作中已經看到了這一點。辦公室某處有一台電視，在那台電視上 可能是圖表或 CloudWatch 警報之類的。某些警報會每隔一段時間觸發並顯示在該電視上， 你會被告知忽略它，因為這沒什麼大不了的。
“我們只是想知道這種情況是否發生太多了”通常是 報導什麼。

最終，這些開始滲透到隨叫隨到的警報中，它會傳給您。你會再次被告知它們是信息豐富的，通常是 由擁有該服務的團隊提供。隨著時間的流逝，警報應該告訴您的內容變得不清楚， 只有新人會得到關於警報是否重要的​​混亂信息。你最終會擁有
中斷，因為“正常”警報將在異常情況下觸發，導致人們將頁面靜音並繼續 接著睡。

我已經這樣做了，我什至為系統辯護，理由是“寫警報的人肯定有 背後的某種意圖”。我應該站在“把它全部拆掉重新開始”的一邊，但我選擇了一個 奇怪的中間地帶。多年前這對我來說是錯誤的決定，而今天對你來說也是錯誤的決定。

反而....

如果警報尋呼某人，則必須是系統永遠無法自行恢復的情況。它需要是 嚴重的，它不可能是應用程序設計中內置的失敗。一個例子是“ 好吧，有時我們的服務需要重新啟動，只需通過 SSH 登錄並重新啟動它”。不，這不是喚醒我的可接受的理由
向上。如果您的服務就這樣消失了，請想辦法將其恢復。

不要讓垃圾警報慢慢逐漸污染您的生活，並隨時宣布破產 如果它們開始發臭，則會在平台中發出警報。如果系統每天向您發送 600 次電子郵件，則它不起作用。如果有一個 鬆弛的通道被垃圾污染了，沒有人進入那裡，它不能作為警報系統工作。這不是如何
人類的注意力是有效的，你不能不斷地向某人發送“非警報”的垃圾郵件，然後突然期望他們小心 解析警報電子郵件的每個字符串並意識到“等待這個不同”。

# 不要在 python 中編寫內部 cli 工具

我會保持這個簡短而甜蜜的。

沒有人知道如何正確安裝和打包 Python 應用程序。如果你用 Python 編寫一個內部工具，它要么需要 完全可移植或僅用 Go 或 Rust 編寫。當人們努力安裝時，讓自己省去很多心痛 正確的事情。

Topic - DevOps, Programming, Work
