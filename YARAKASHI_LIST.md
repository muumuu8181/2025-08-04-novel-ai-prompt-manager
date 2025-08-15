# 🚨 やらかしリスト - Firebase統合プロジェクト

## 📝 概要
Novel AI Prompt Managerプロジェクトでの失敗事例とその対策をまとめたリスト

---

## ❌ やらかし一覧

### #001 - Firebase認証でauth.emailを使用してPERMISSION_DENIED発生
**日付**: 2025-08-15  
**担当者**: Claude AI Assistant  
**内容**: 
- Firebase Realtime Databaseのルールが `$uid === auth.uid` なのに、コードで `auth.email` を使用
- 結果：全てのDB操作でPERMISSION_DENIEDエラー

**原因**:
```javascript
// ❌ 間違い
const userId = currentUser.email.replace(/[.#$[\]]/g, '_');
const testRef = ref(database, `test_users/${userId}/simple_test`);
```

**修正**:
```javascript
// ✅ 正解
const userId = currentUser.uid; // Firebase UIDを使用
const testRef = ref(database, `users/${userId}/novel_ai_prompts/test_data`);
```

**教訓**: 
- Firebaseルールを事前確認する
- `auth.uid` vs `auth.email` の違いを理解する
- 既存のDB構造を把握してからコーディングする

---

### #002 - 重複フォルダ構造で混乱発生
**日付**: 2025-08-15  
**担当者**: 前任者AI  
**内容**:
- core/, dev/, src/ の3つのバージョンが並存
- どれがメインか不明確で開発効率低下

**修正**:
- `old/` フォルダを作成してdev/, src/を移動
- `core/` をメイン作業版として統一

**教訓**:
- プロジェクト構造は常にクリーンに保つ
- バージョン管理は適切に行う

---

### #003 - ES6モジュールスコープでグローバル関数が未定義
**日付**: 2025-08-15  
**担当者**: Claude AI Assistant  
**内容**:
- `<script type="module">` 内で定義した関数がonclick属性から呼び出せない
- `ReferenceError: testLogin is not defined`

**原因**:
```javascript
// ❌ ES6モジュール内の関数は外部からアクセス不可
function testLogin() { ... }
```

**修正**:
```javascript
// ✅ windowオブジェクトに明示的に追加
window.testLogin = async function() { ... };
```

**教訓**:
- ES6モジュールスコープを理解する
- HTML属性からの関数呼び出しはグローバルスコープが必要

---

## 📊 統計

- **総やらかし数**: 3件
- **解決済み**: 3件
- **未解決**: 0件
- **最終更新**: 2025-08-15

---

## 🎯 今後の改善策

1. **事前調査の徹底**
   - Firebase設定とルールの確認
   - 既存コード構造の把握

2. **段階的実装**
   - 小さな機能から順次テスト
   - 各段階での動作確認

3. **ドキュメント化**
   - 設定変更の記録
   - 失敗事例の共有

---

*このリストはFirebaseで管理予定*