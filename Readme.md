# Wallpaper Generator 🖼️

A beautiful, responsive web application that generates high-quality wallpapers using the Pexels API. Built with HTML, CSS, and Vanilla JavaScript.

## 🌟 Features

- **Curated Gallery**: Instantly loads a curated list of high-quality images on startup.
- **Search Functionality**: Search for any topic (e.g., "Nature", "Cars", "Abstract") to find specific wallpapers.
- **Load More**: Infinite-scroll style "More" button to fetch additional results without reloading.
- **Responsive Design**: A masonry-style grid layout that adapts perfectly to desktop, tablet, and mobile screens.
- **Download Links**: Direct links to download high-resolution original images.

## 🛠️ Built With

- **HTML5**: Semantic structure.
- **CSS3**: Custom styling with CSS Grid and Flexbox for layout.
- **JavaScript (ES6+)**: Async/Await for API fetching, DOM manipulation for dynamic rendering.
- **Pexels API**: Source for high-quality, free stock photos.

## 🚀 How to Run

1.  **Clone the repository** (or download usage files):
    ```bash
    git clone 
    ```
2.  **Open `index.html`**:
    Simply double-click `index.html` to open it in your web browser. No backend server or installation required!

3.  **Add your API Key**:
    - Create a file named `apikey.js` in the project folder.
    - Add the following line:
      ```javascript
      const apikey = "YOUR_PEXELS_API_KEY_HERE";
      ```
    - Save the file. The app will now work!


## 🧠 What I Learned

This project was part of my **100 Days of Code** challenge (Day 55). Key concepts practiced:
- **API Integration**: Authenticating and fetching data from a third-party API (Pexels).
- **Asynchronous JavaScript**: Using `async/await` to handle promises cleaner than `.then()`.
- **DOM Manipulation**: Dynamically creating and appending HTML elements based on JSON data.
- **State Management**: tracking `page` numbers and `search` queries to implement pagination ("Load More" feature).

---

> **Note**: This project uses a free API key from Pexels. 
