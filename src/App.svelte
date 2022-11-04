<script>
import { onMount } from "svelte";



    let titles = [
        '#',
        'Coin',
        'Price',
        'Price Change',
        '24h Volume',
    ]
    let coins = [];
    let filteredCoins = []
    let ref = null

    const loadCoins = async () => {
        const res = await fetch(
            'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false',
        );
        const data = await res.json();
        coins = data;
        filteredCoins = data;
    };
    loadCoins();

    const searchCoins = (value) => {
        filteredCoins = coins.filter(coins => coins.name.toLowerCase().includes(value.toLowerCase())
        ||
        coins.symbol.toLowerCase().includes(value.toLowerCase())
        )
    }

    onMount (()=> {
        ref.focus()
    });
</script>

<main class="bg-cyan-800 text-white ">



<h2 class="text-dark p-2" >Buscar criptomoneda</h2>

<input type="text" class="form-control bg-dark text-white rounded-0 border-0 my-4"
placeholder="Search Crypto"

on:keyup={({target: {value}}) => searchCoins(value) }
bind:this={ref}
>

    <table class="table bg-light">
        <thead class="bg-black text-white">
          <tr>
                {#each titles as tit}
                
                <th scope="col">
                  {tit}
              </th>
                           
              {/each}
          </tr>
        </thead>
        <tbody >
            {#each filteredCoins as coin, i}
          <tr>
              
              <td class="text-muted">{i + 1}</td>
              <td >
                  <img src={coin.image} style="width: 2rem" class="img-fluid me-2" alt={coin.name}>
                  <span>{coin.name}</span>
                  <span class="text-muted text-uppercase ms-2">{coin.symbol}</span>
              </td>
            <td >$ {coin.current_price.toLocaleString()}</td>
            <td class="{coin.price_change_percentage_24h>0 ? "bg-success" : "bg-danger"} text-white rounded text-center">
                {coin.price_change_percentage_24h>0 ? "▲" : "▼"} 
                 
                {coin.price_change_percentage_24h} %</td>
            <td >$ {coin.total_volume.toLocaleString()}</td>
            
        </tr>        
        {/each}
      
         
        </tbody>
      </table>
</main>

<style >

</style>
