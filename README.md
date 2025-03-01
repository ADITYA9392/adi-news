npm install express ejs body-parser fs
<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><%= article.title %></title>
</head>
<body>
    <h1><%= article.title %></h1>
    <p><%= article.content %></p>
    <p>तारीख: <%= article.date %></p>
    <a href="/">होम पेज पर वापस जाएं</a>
</body>
</html>
const express = require("express");
const fs = require("fs");
const bodyParser = require("body-parser");

const app = express();
const PORT = process.env.PORT || 3000;

app.set("view engine", "ejs");
app.use(express.static("public"));
app.use(bodyParser.json());

// Load news data from JSON file
const newsData = JSON.parse(fs.readFileSync("./data/news.json", "utf8"));

// Home Page
app.get("/", (req, res) => {
    res.render("index", { news: newsData });
});

// Single News Page
app.get("/news/:id", (req, res) => {
    const newsId = req.params.id;
    const article = newsData.find(n => n.id == newsId);
    if (!article) return res.status(404).send("News Not Found!");
    res.render("news", { article });
});

// Start Server
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
[
    {
        "id": 1,
        "title": "भारत में नई टेक्नोलॉजी का आगमन",
        "content": "यह भारत में टेक्नोलॉजी इंडस्ट्री के लिए एक बड़ा कदम है।",
        "date": "2025-02-28"
    },
    {
        "id": 2,
        "title": "सरकार ने नए कानून बनाए",
        "content": "नए नियमों के अनुसार, सभी नागरिकों को इसका पालन करना होगा।",
        "date": "2025-02-28"
    }
]


