<script>
function handleSubmit(event) {
    event.preventDefault();
    let form = event.target;
    let data = new FormData(form);

    let params = [];
    for (let [name, value] of data) {
        params.push(name + "=" + encodeURIComponent(value));
    }
    let url = form.action + "?" + params.join("&");

    let a = document.createElement("a");
    a.href = url;
    a.click();
}
</script>

<svelte:head>
  <title>Contact me</title>
</svelte:head>

<h1>Contact</h1>

<form action="mailto:hoga@mit.edu" method="GET" on:submit={handleSubmit}>
    <label>
        Subject:
        <input name="subject" value="Test Subject">
    </label>
    <label>
        Message:
        <textarea name="body">This is a test message.</textarea>
    </label>
    <button>Submit</button>
</form>
