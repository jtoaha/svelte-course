<script>
  // how to create props in Svelte
  // - these props will be required if default value is not set
  export let initialCount; // required
  export let maxCount = 10; // optional 
    // - even if you do not want a value set for an optional prop, you need to still specify
    // export let maxCount = undefined;


  // We also have access to several variables
  console.log($$props, $$restProps); // $$Props is an object that contains all the props passed to this component
  // $$restProps is an object that contains all the props passed to this component that are not defined in the script tag (basically extra props)
  // only use when necessary, bc can cause performance issues in the svelte compiler
  


  let count = initialCount || 0;
  const increment = () => {
     if(count < maxCount) count++; 
    //  if(count === maxCount) return;
    //  count++;  // equivalent to above
  };
  let string = `Non-reactive: String: You have clicked ${count} times`; // the script here is only run once when you intialize component,
        // hence why string value doesn't get updated when count is incremented
        // to make something update, you have to use reactive statements
        // can be solved by adding state directly
  let stringReactive;
  // example of reactive statement - runs one time when component is initialized, and then again whenever dependencies change
  // - any value we depend on inside the statement will cause the statement to rerun
  // -- for example, here this will rerun  when count changes
  // - can be a simple variable statement, or can be a whole code block

  // Both are equivalent below
  $: {
    stringReactive = `Reactive: You have clicked ${count} times`;
  }

  $: stringReactive = `Reactive: You have clicked ${count} times`;

  $: if(count > 5) {
    console.log(`count is ${count}`);
  }

  // Sometimes reactive statements might not work as you expect

  let count1 = 0;
  let count2 = 0;
  const increment1 = () => count1++;
  const increment2 = () => count2++;
  let stringDisplay;
  $: stringDisplay = `Count1 ${count1} + Count2 ${count2} =  ${count1 + count2}`;


  const getTotalBest = (x, y) => {
    return x + y;
  }

  const getTotalBad = () => {
    return count1 + count2;
  } 

  $: stringDisplay = `The count total is ${getTotalBad()}`; // not going to work
  $: stringDisplay = `The count total is ${getTotalBest(count1, count2)}`; // this will get updated version fo count since on right hand side and svelte knows to get updated value

  // be aware of order in which you write reactive statements  

  function setCount2(x) {
    count2 = x;
  }
  // -- this order will not display updated count2 value. count2 is updated after its evaluated by svelte, so doesn't 
  // $: string = `Count 2 is ${count2}`;
  // $: setCount2(count1); 

  // -- this order will display updated count2 value, since svelte knows that count2 was updated
  $: setCount2(count1); 
  $: string = `Count 2 is ${count2}`;


</script>



<h1>{string}</h1> <!-- this indirectly accesses state, be careful bc can display stale value -->
<h1> You have clicked {count} times.</h1> <!-- this directly accesses state -->
<h1>{stringReactive}</h1>

<h2>Counter: {count}</h2>
<input type="button" bind:value={count} on:click={increment} />
<br>

<button on:click={increment} > <!-- in Svelte, add on directive-->
  <h2>Counter: {count}</h2>
</button>
<br>

 <!-- <button on:click={()=> {}} > you can add inline function in Svelte as well
  <h1>Counter: {count}</h1>
</button> -->

<br> 

<hr>

<button on:click={increment1}>Clicks {count1}</button>
<button on:click={increment2}>Clicks {count2}</button>
<h1>{stringDisplay}</h1>
<style>
  button, input {
  padding: 2rem;
  }
</style>