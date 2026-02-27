<svelte:head>
  <title>Contact me</title>
</svelte:head>

    <h1>Contact</h1>
        
    <form action="mailto:hoga@mit.edu" method="GET">
    <label>
        Email:
        <input name="email" type="email" value="test@example.com">
    </label>
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
    <link rel="stylesheet" href="../style.css">