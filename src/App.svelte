
<script>
    import Counter from "./lib/Counter.svelte";
    const maxCount = 6; // 
    // you can also spread props
    const props = {
        maxCount: 10,
        initialCount: 5
    }
</script>

<!-- how to pass props to svelte components. Then in Counter component will use export keyword-->
<!-- <Counter initialCount={3} maxCount={10} /> -->
<!-- <Counter initialCount={3} {maxCount} /> -->
<Counter {...props} x="y"/>

<style>

</style>