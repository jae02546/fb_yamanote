<script>
    import { onMount } from "svelte";
    let stations = [
      "東京", "神田", "秋葉原", "御徒町", "上野", "鶯谷", "日暮里", "西日暮里", "田端",
      "駒込", "巣鴨", "大塚", "池袋", "目白", "高田馬場", "新大久保", "新宿", "代々木",
      "原宿", "渋谷", "恵比寿", "目黒", "五反田", "大崎", "品川", "高輪ゲートウェイ", "田町",
      "浜松町", "新橋", "有楽町"
    ];
    let shuffledStations = [];
    let correct = false;
  
    onMount(() => {
      shuffledStations = shuffle([...stations]);
    });
  
    function shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }
  
    function checkOrder() {
      correct = JSON.stringify(stations) === JSON.stringify(shuffledStations);
    }
  </script>
  
  <style>
    ul {
      list-style-type: none;
      display: flex;
      flex-wrap: wrap;
    }
    li {
      padding: 10px;
      margin: 5px;
      background-color: #4caf50;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
  
  <h1>山手線ゲーム</h1>
  <ul>
    {#each shuffledStations as station, i}
      <li
        draggable="true"
        on:dragstart="{(e) => e.dataTransfer.setData('text/plain', i)}"
        on:dragover="{(e) => e.preventDefault()}"
        on:drop="{(e) => {
          e.preventDefault();
          const from = e.dataTransfer.getData('text/plain');
          [shuffledStations[from], shuffledStations[i]] = [shuffledStations[i], shuffledStations[from]];
        }}"
      >
        {station}
      </li>
    {/each}
  </ul>
  <button on:click="{checkOrder}">順番を確認する</button>
  {#if correct}
    <p>正解です！おめでとうございます！</p>
  {:else}
    <p>並べ替えが完了したら、「順番を確認する」ボタンを押してください。</p>
  {/if}
  
  