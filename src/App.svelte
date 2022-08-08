<script>

  let coins = [];
  let headings = ["#", "Coin", "Price", "Price Change", "24h Volume"];
  let searching = "";
  let filters = [];

  let ref = null;

  const getCrypto = async () => {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=eur&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    coins = data;
    filters = data;
  };
  getCrypto();

  const searchCoin = (value) => {
    filters = coins.filter(
      (coin) =>
        coin.name.toLowerCase().includes(value) ||
        coin.symbol.toLowerCase().includes(value)
    );
  };

</script>

<main class="container p-4"> 

  <h1 class="text-center">Vite + Svelte + Crypto</h1>
  <input
    type="text"
    class="form-control bg-dark text-white rounded-0 border-0 my-4"
    bind:value={searching}
    on:keyup={({ target: { value } }) => searchCoin(value)}
    bind:this={ref}
    placeholder="Search crypto..."
  />
  <table class="table table-dark table-hover">
    <thead>
      <tr>
        {#each headings as heading}
          <th>{heading}</th>
        {/each}
      </tr>
    </thead>
    <tbody>
      {#each filters as coin, i}
        <tr class="fila">
          <td class="text-muted">{i}</td>
          <td>
            <img
              src={coin.image}
              alt={coin.name}
              style="width: 2rem"
              class="img-fluid"
            />
            <span>
              {coin.name}
            </span>
            <span class="ms-3 text-muted text-uppercase">
              {coin.symbol}
            </span>
          </td>
          <td>
            €{coin.current_price.toLocaleString()}
          </td>
          <td
            class={coin.price_change_percentage_24h > 0
              ? "text-primary"
              : "text-danger"}
          >
            {coin.price_change_percentage_24h > 0 ? "🔼 " + coin.price_change_percentage_24h.toLocaleString() : "🔻 " + coin.price_change_percentage_24h.toLocaleString() } %
          </td>
          <td>
            <div class="volume">
              €{coin.total_volume.toLocaleString()}
            </div>
            
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
</main>

<style>
</style>