# ナレッジ検索

### 1 定義

ナレッジベースからユーザーの質問に関連するテキスト内容を検索し、それを下流のLLMノードのコンテキストとして使用することができます。

***

### 2 シナリオ

一般的なシナリオ：外部データ/ナレッジに基づくAI質問応答システム（RAG）を構築。RAGの[基本概念](../../../learn-more/extended-reading/retrieval-augment/)についてもっと知る。

下図は最も基本的なナレッジベース質問応答アプリケーションの例です。このプロセスの実行ロジックは、ユーザーの質問がLLMノードに渡される前に、ナレッジ検索ノードでユーザーの質問に最も関連するテキスト内容を検索し、召喚することです。その後、LLMノード内でユーザーの質問と検索されたコンテキストを一緒に入力し、LLMが検索内容に基づいて質問に答えるようにします。

<figure><img src="../../../../img/jp-knowledge-retrieval.png" alt=""><figcaption><p>ナレッジベース質問応答アプリケーションの例</p></figcaption></figure>

***

### 3 どのように設定するか

<figure><img src="../../../../img/jp-knowledge-retrieval-setting.png" alt=""><figcaption><p>ナレッジ検索の設定</p></figcaption></figure>

**設定プロセス：**

1. クエリ変数を選択し、ナレッジベース内の関連するテキストセグメントを検索するための入力として使用します。一般的な対話型アプリケーションでは、開始ノードの`sys.query`をクエリ変数として使用します。
2. 検索するナレッジベースを選択します。オプションとして選択可能なナレッジベースは、Difyナレッジベース内で事前に[作成](../../knowledge-base/create-knowledge-and-upload-documents.md#id-1-chuang-jian-zhi-shi-ku)する必要があります。
3. [リコールモード](../../../learn-more/extended-reading/retrieval-augment/retrieval.md)と[ナレッジベース設定](../../knowledge-base/knowledge-and-documents-maintenance.md#id-8-zhi-shi-ku-she-zhi)を設定します。
4. 下流ノードを接続し設定します。一般的にはLLMノードです。

**出力変数**

<figure><img src="../../../../img/jp-knowledge-retrieval-output.png" alt="" width="272"><figcaption><p>出力変数</p></figcaption></figure>

ナレッジ検索の出力変数`result`は、ナレッジベースから検索された関連テキストセグメントです。この変数のデータ構造には、セグメント内容、タイトル、リンク、アイコン、メタデータ情報が含まれています。

**下流ノードの設定**

一般的な対話型アプリケーションでは、ナレッジベース検索の下流ノードは通常LLMノードであり、ナレッジ検索の**出力変数**`result`はLLMノード内の**コンテキスト変数**に関連付けられて設定されます。関連付け後、プロンプトの適切な位置に**コンテキスト変数**を挿入することができます。

{% hint style="info" %}
コンテキスト変数は、LLMノード内で定義された特殊な変数タイプで、プロンプト内に外部検索されたテキスト内容を挿入するために使用されます。
{% endhint %}

ユーザーが質問すると、関連するテキストがナレッジ検索で召喚された場合、そのテキスト内容がコンテキスト変数の値としてプロンプトに挿入され、LLMが質問に答えます。関連するテキストが検索されなかった場合、コンテキスト変数の値は空となり、LLMは直接ユーザーの質問に答えます。

<figure><img src="../../../../img/jp-knowledge-retrieval-llm.png" alt=""><figcaption><p>下流LLMノードの設定</p></figcaption></figure>

この変数は、LLMが質問に答える際のプロンプトコンテキストとして外部ナレッジの参照に使用されるだけでなく、そのデータ構造にセグメントの引用情報が含まれているため、アプリケーション側の[**引用と帰属**](../../knowledge-base/retrieval-test-and-citation.md#id-2-yin-yong-yu-gui-shu)機能もサポートします。