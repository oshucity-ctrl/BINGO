<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>競馬ビンゴ 3x3（3人分ボーナス対応版）</title>
<style>
  body { font-family: sans-serif; text-align: center; padding: 20px; }
  .bingo-container { display: flex; justify-content: center; gap: 50px; margin-top: 20px; flex-wrap: wrap; }
  .bingo-board { display: flex; flex-direction: column; align-items: center; }
  .bingo { display: grid; grid-template-columns: repeat(3, 100px); gap: 10px; margin-bottom: 0; } /* 下のボーナスと接続 */
  .cell {
    padding: 0;
    border: 2px solid #333;
    cursor: pointer;
    user-select: none;
    transition: 0.2s;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    height: 60px;
    width: 100px;
    box-sizing: border-box;
    font-weight: bold;
    line-height: 1; /* 文字位置安定 */
  }
  .cell.marked { background-color: #4caf50; color: white; }
  .cell.bingo { background-color: #f44336; color: white; } /* ビンゴ列が赤色でも中央揃え */
  h2 { margin-bottom: 10px; }
  .bonus { margin-top: 5px; font-size: 16px; font-weight: bold; }
  .rules { text-align: left; display: inline-block; margin-top: 30px; font-size: 16px; line-height: 1.6; }
</style>
</head>
<body>

<h1>競馬ビンゴ 3x3（3人分ボーナス対応版）</h1>

<div class="bingo-container" id="bingoContainer"></div>

<div class="rules">
  <h2>ルール説明</h2>
  <ul>
    <li>縦、横、斜めの1列ビンゴごとに1万円のボーナス</li> 
    <li>すべてのマスが埋ると8列×1万円に加えて、さらに10万円のボーナスが加算され、合計18万円のボーナスとなる</li>
  </ul>
</div>

<script>
// 3x3固定のビンゴ内容（中央は万馬券）
const fixedBoard = [
  "単勝", "複勝", "馬連",
  "馬単", "万馬券", "三連複",
  "三連単", "枠連", "ワイド"
];

const players = ["小野大","睦夫","友紀"];

function renderFixedBingoBoards() {
  const container = document.getElementById('bingoContainer');
  container.innerHTML = '';

  players.forEach(player => {
    const boardDiv = document.createElement('div');
    boardDiv.className = 'bingo-board';

    const title = document.createElement('h2');
    title.textContent = player;
    boardDiv.appendChild(title);

    const grid = document.createElement('div');
    grid.className = 'bingo';

    const cells = [];
    fixedBoard.forEach((item, idx) => {
      const cell = document.createElement('div');
      cell.className = 'cell';
      cell.textContent = item;
      cell.dataset.index = idx;
      cell.onclick = () => {
        cell.classList.toggle('marked');
        checkBingo(cells, bonusDisplay);
      };
      grid.appendChild(cell);
      cells.push(cell);
    });
    boardDiv.appendChild(grid);

    // 個別ボーナス表示
    const bonusDisplay = document.createElement('div');
    bonusDisplay.className = 'bonus';
    bonusDisplay.textContent = `ビンゴ成立列数：0列 | 合計ボーナス：0万円`;
    boardDiv.appendChild(bonusDisplay);

    container.appendChild(boardDiv);
  });
}

// ビンゴ判定（縦・横・斜め）
function checkBingo(cells, bonusDisplay) {
  const lines = [
    [0,1,2], [3,4,5], [6,7,8], // 横
    [0,3,6], [1,4,7], [2,5,8], // 縦
    [0,4,8], [2,4,6]           // 斜め
  ];

  // 既存のビンゴスタイルをリセット
  cells.forEach(cell => cell.classList.remove('bingo'));

  let bingoCount = 0;
  lines.forEach(line => {
    if (line.every(i => cells[i].classList.contains('marked'))) {
      line.forEach(i => cells[i].classList.add('bingo'));
      bingoCount++;
    }
  });

  // 全マスがマークされているか確認
  const allMarked = cells.every(cell => cell.classList.contains('marked'));

  // ボーナス計算
  let bonus = bingoCount * 1;
  if (allMarked) bonus += 10;

  // 個別表示更新
  bonusDisplay.textContent = `ビンゴ成立列数：${bingoCount}列 | 合計ボーナス：${bonus}万円`;
}

// 初期表示
renderFixedBingoBoards();
</script>

</body>
</html>
