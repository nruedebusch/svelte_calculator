<!-- script -->
<script lang="ts">

<!-- Numbers -->

  const numbers = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "."];
  const operations = ["+", "-", "*", "/", "="];

  let selectedOperation = "";
  let display = "";
  let firstNumber = "";
  let secondNumber = "";
  let isDisplayingResults = false;

  const handleOperationClick = (operation: string) => {
    if (!firstNumber) return;
    if (operation === "=") {
      if (!secondNumber) return;
      const firstNum = parseFloat(firstNumber);
      const secondNum = parseFloat(secondNumber);

      let result = 0;
      switch (selectedOperation) {
        case "/":
          result = firstNum / secondNum;
          break;
        case "*":
          result = firstNum * secondNum;
          break;
        case "+":
          result = firstNum + secondNum;
          break;
        case "-":
          result = firstNum - secondNum;
          break;
      }

      if (!Number.isInteger(result)) {
        display = result.toFixed(2);
      } else {
        display = result.toString();
      }
      isDisplayingResults = true;
    }
    selectedOperation = operation;
  };

  const handleClear = () => {
    firstNumber = "";
    secondNumber = "";
    selectedOperation = "";
    display = "";
    isDisplayingResults = false;
  };

  const handleNumberClick = (number: string) => {
    if (isDisplayingResults) {
      handleClear();
    }
    if (display === "" && number === "0") return;
    if (number === "." && display.includes(".")) return;
    if (display === "" && number === ".") {
      return (display = "0.");
    }

    if (!selectedOperation) {
      if (display === "" && number === ".") {
        firstNumber = "0.";
        return (display = firstNumber);
      }
      firstNumber = `${firstNumber}${number}`;
      return (display = firstNumber);
    } else {
      if (display === "" && number === ".") {
        secondNumber = "0.";
        return (display = secondNumber);
      }
      secondNumber = `${secondNumber}${number}`;
      return (display = secondNumber);
    }
  };
</script>

<!-- main -->
<main>
  <div class="calculator">
    <div class="results">
      {display}
    </div>
    <div class="digits">
      <div class="numbers">
        <button class="btn btn-xlg" on:click={handleClear}>C</button>
        {#each numbers as number (number)}
          <button
            class={`btn ${number === "0" ? "btn-lg" : null}`}
            on:click={() => handleNumberClick(number)}>{number}</button
          >
        {/each}
      </div>
      <div class="operations">
        {#each operations as operation (operation)}
          <button
            class={`btn ${
              operation === selectedOperation ? "btn-silver" : "btn-orange"
            }`}
            on:click={() => handleOperationClick(operation)}>{operation}</button
          >
        {/each}
      </div>
    </div>
  </div>
</main>


<style>
  main {
    width: 100%;
    height: 100vh;
    margin: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: darkslategrey;
  }

  .calculator {
    background-color: rgb(28, 28, 28);
    width: 240px;
    padding: 15px;
    border-radius: 7px;
  }
  .digits {
    display: flex;
  }
  .operations {
    display: flex;
    flex-direction: column;
  }
  .numbers {
    display: flex;
    flex-wrap: wrap;
    width: 200px;
  }
  .btn {
    width: 50px;
    height: 50px;
    border-radius: 100px;
    background-color: rgb(114, 113, 113);
    font-size: 20px;
    font-weight: bold;
    color: white;
    margin: 5px;
    border: none;
  }
  .btn-lg {
    width: 110px;
  }
  .btn-orange {
    background-color: orange;
  }
  .btn-silver {
    background-color: silver;
  }
  .btn-xlg {
    width: 170px;
  }
  .results {
    height: 60px;
    color: white;
    font-size: 50px;
    display: flex;
    flex-direction: row-reverse;
    margin-right: 10px;
    border-radius: 3.5px;
  }

</style>
