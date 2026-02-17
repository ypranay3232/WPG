# Wallpaper Generator - Project Notes 📝

Welcome to the detailed breakdown of the Wallpaper Generator project! This guide is designed to explain every part of the code so that even a complete beginner can understand how it works.

## 1. HTML (`index.html`) - The Skeleton 💀

The HTML file sets up the structure of our webpage.

```html
<!DOCTYPE html>
<html lang="en">
```
- `<!DOCTYPE html>`: Tells the browser this is an HTML5 document.
- `<html lang="en">`: The root element, specifying the language is English.

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Wallpaper Generator</title>
    <link rel="stylesheet" href="styles.css">
    <!-- Fonts ... -->
    <script defer src="app.js"></script>
</head>
```
- **Meta tags**: Set character encoding (UTF-8) and ensure the page looks good on mobile (`viewport`).
- **Title**: What you see in the browser tab.
- **Link**: Connects our CSS file (`styles.css`) to style generic HTML elements.
- **Script**: Connects our JavaScript file (`app.js`). The `defer` attribute is crucial—it tells the browser to wait until the HTML is fully parsed before running the script. This ensures our HTML elements exist before JavaScript tries to access them.

```html
<body>
    <header>
        <h2><a id="logo" href="./index.html">WPG</a></h2>
        <form class="search-form">
            <input type="text" class="search-input" name="search">
            <button type="submit" class="submit-btn">Search</button>
        </form>
    </header>
```
- **Header**: Contains the Logo and the Search bar.
- **Form**: We use a `<form>` for the search bar. This allows us to listen for the 'submit' event (like pressing Enter).

```html
    <main>
        <div class="gallery"> </div>
    </main>
```
- **Main**: The main content area.
- **.gallery**: An empty `div`. This is our container! Our JavaScript will dynamically create images and put them *inside* this box.

```html
    <div class="nav-button">
        <button class="more">More</button>
    </div>
</body>
```
- **Load More**: A button at the bottom to load more images.

---

## 2. CSS (`styles.css`) - The Styling 🎨

CSS makes our page look good.

- **Global Reset**:
  ```css
  * { margin: 0; padding: 0; box-sizing: border-box; }
  ```
  Removes default spacing so we have full control. `box-sizing: border-box` makes sizing elements much easier (padding doesn't add to width).

- **Gallery Grid**:
  ```css
  .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
      gap: 2rem;
  }
  ```
  - `display: grid`: Turns the container into a grid.
  - `grid-template-columns`: The magic part!
    - `repeat(auto-fill, ...)`: Automatically create as many columns as fit in the screen.
    - `minmax(350px, 1fr)`: Each column must be at least `350px` wide, but can stretch (`1fr`) to fill extra space. This makes it responsive without media queries!

- **Image Overlay**:
  ```css
  .gallery-info {
      /* ... positioning ... */
      opacity: 0;
      transition: opacity 0.3s ease;
  }
  .gallery-img:hover .gallery-info {
      opacity: 1;
  }
  ```
  The info text is hidden (`opacity: 0`) by default. When you hover over the image container (`.gallery-img:hover`), it becomes visible (`opacity: 1`).

---

## 3. JavaScript (`app.js`) - The Logic 🧠

This is where the magic happens. Let's break it down line by line.

### Setup and Variables

```javascript
const apikey = '...'; // Your Secret Key
```
- We store our Pexels API key in a variable to use it later when requesting data.

```javascript
const gallery = document.querySelector('.gallery');
const searchInput = document.querySelector('.search-input');
const form = document.querySelector('.search-form');
const more = document.querySelector('.more');
```
- We "grab" elements from our HTML using `querySelector` so we can manipulate them.

```javascript
let searchValue;
let page = 1;
let fetchLink;
let currentSearch;
```
- **State Variables**: These keep track of what the app is doing.
  - `searchValue`: Stores what the user types.
  - `page`: Tracks which page of results we are on (starts at 1).
  - `fetchLink`: The specific URL we are currently fetching from.
  - `currentSearch`: Remembers the *last* successful search term (important for "Load More").

### Event Listeners

```javascript
searchInput.addEventListener('input', updateInput);
```
- Listens for every keystroke in the search box and runs `updateInput`.

```javascript
form.addEventListener('submit', (e) => {
    e.preventDefault();
    currentSearch = searchValue;
    searchPhotos(searchValue);
});
```
- Listens for the form submission (pressing Enter or clicking Search).
- `e.preventDefault()`: **Crucial!** Prevents the page from refreshing (the default behavior of forms).
- We save the search term and call `searchPhotos`.

```javascript
more.addEventListener('click', loadMore);
```
- Listens for clicks on the "More" button.

### Helper Functions

```javascript
function updateInput(e) {
    searchValue = e.target.value;
}
```
- Simply updates our `searchValue` variable with whatever is currently in the input field.

### The API Fetcher

```javascript
async function fetchApi(url) {
    const dataFetch = await fetch(url, {
        method: "GET",
        headers: {
            Accept: "application/json",
            Authorization: apikey
        }
    });
    const data = await dataFetch.json();
    return data;
}
```
- **`async/await`**: Makes working with asynchronous data (like fetching from the web) look like synchronous code.
- **`fetch(url, options)`**: The command to talk to the server (Pexels).
- **`headers`**: We pass our `apikey` here so Pexels knows we are allowed to ask for photos.
- **`.json()`**: Converts the raw response into a JavaScript object we can use.

### Generating HTML

```javascript
function generatePictures(data) {
    data.photos.forEach(photo => {
        // ... creation logic
    });
}
```
- Takes the data (list of photos) and loops through each one using `forEach`.
- For each photo, it creates a `div`, fills its HTML with the image and info, and appends it to the `gallery`.

### Main Logic Flows

1.  **Initial Load (`curatedPhotos`)**:
    ```javascript
    async function curatedPhotos() {
        fetchLink = "https://api.pexels.com/v1/curated?per_page=15&page=1";
        const data = await fetchApi(fetchLink);
        generatePictures(data);
    }
    ```
    - Called at the very bottom of the file to start the app. Fetches "Curated" (popular) photos.

2.  **Searching (`searchPhotos`)**:
    ```javascript
    async function searchPhotos(query) {
        clear(); // Wipes the current gallery
        fetchLink = `https://api.pexels.com/v1/search?query=${query}...`;
        const data = await fetchApi(fetchLink);
        generatePictures(data);
    }
    ```
    - Clears old photos, constructs a *search* URL, fetches data, and renders it.

3.  **Loading More (`loadMore`)**:
    ```javascript
    async function loadMore() {
        page++; // Go to next page
        if (currentSearch) {
            // If we were searching, keep searching that term
            fetchLink = `https://api.pexels.com/v1/search?query=${currentSearch}...&page=${page}`;
        } else {
            // Otherwise, keep getting curated photos
            fetchLink = `https://api.pexels.com/v1/curated?...&page=${page}`;
        }
        const data = await fetchApi(fetchLink);
        generatePictures(data);
    }
    ```
    - Increments the `page` counter.
    - Checks `currentSearch` to decide *what* to load more of (searched items or curated items).
    - Fetches the new page and *appends* it (doesn't clear the old ones).

---

## Summary
1.  **HTML** provides the structure (Input, Button, Empty Gallery div).
2.  **CSS** arranges it into a grid and makes it pretty.
3.  **JS** waits for events (typing, clicking), talks to the Pexels API, gets data, and creates HTML elements to put into the Gallery div.
