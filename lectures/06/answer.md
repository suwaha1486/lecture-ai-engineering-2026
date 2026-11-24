# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：glossary_search_glossary
- STEP 2 の [tool call]：[tool call]   glossary_search_glossary({'query': '3レベル'})
- STEP 3 の [tool call]：[tool call]   fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
- STEP 4 の [tool call]：[tool call]   aws_aws___search_documentation({'search_phrase': 'Amazon S3 バージョニング とは', 'topics': ['general']})
- 気づき・疑問：STEP 4 の最初の質問（「AWSのAmazon S3とは何ですか？」）では aws のツールが呼ばれず，モデルが自分の知識だけで答えた．「AWSのドキュメントを検索して」と頼むと `aws_aws___search_documentation` が呼ばれた．ツールが使えることと，モデルが使うことは別で，質問の書き方や指示文にも左右されると分かった．また，ツールを増やすと入力トークンも増えた（STEP 2 は約700，STEP 4 は約15,900）．

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎
- やったこと：`app/mcp_server.py` の TODO の下に `server.tool()(tools.get_lecture_summary)` を1行書き足した．エージェント側（`app/mcp_agent.py`，`app/mcp_servers.json`）は変えていない．
- 結果：
  - `--list-tools` で `glossary_get_lecture_summary` が一覧に増えた（ツールは7個から8個）．
  - 「第2回では何を学びますか？」と質問すると，`[tool call] glossary_get_lecture_summary({'lecture': 2})` が呼ばれ，第2回の要点をもとに回答した．モデル応答は2回．
  - `uv run pytest -q` は21件すべて通った．
- 気づき・疑問：エージェントは起動のたびにサーバーへツール一覧を問い合わせるので，サーバーに1行足すだけでエージェント側を変えずに使えた．ツール名は「サーバー名_ツール名」になる．docstring がそのままモデルへの説明になるなら，説明文の書き方で呼ばれ方が変わるのか試したい．
