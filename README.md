# 🐾 Adopt-a-Pet Full Stack Setup 

You’ve been given code snippets to build a **full-stack project**. Your job is to place each code snippet in the correct **file** and make sure the file is saved in the **correct folder**.

Use your **Do Now** to understand **which snippet goes where, and in what order.** Then **create and copy each snippet into the right file** (`index.js`, `pets.ejs`, `petScript.js`, or `pets.css`). Don’t forget to save the files in the correct folder if applicable (e.g., `views` or `public`)!

---

💡 **Don’t forget to:**

- Run `npm install` to install your dependencies (`express`, `mongoose`, and `ejs`)
- Use `node index.js` to start your server and test if your app works
- Refer back to the **U5L19 Code Along** if you are stuck. 

---

✅ **Once your app is working**:
> Look at the [Delete Button Slides](https://docs.google.com/presentation/d/1cs0Nl4-AT4v6DWS3QDxJUARxR6YbCWPffCIA_uzaStQ/edit#slide=id.g35282b9ae53_0_1073) and follow the steps to add **DELETE button functionality** to remove pets from the page. 

#### __**This will be graded as today's exit ticket.**__

---

### ✅ Code SNIPPETS

---

```
<section class="form-section">
  <h2>Add a New Pet 💌</h2>
  <form>
    <input type="text" name="name" placeholder="Pet's name 🐾" required>
    <input type="text" name="emoji" placeholder="Emoji 🐶" required>
    <input type="number" name="age" placeholder="Age 🕒" required min="0">
    <label>
      <input type="checkbox" name="adopted">
      Already Adopted?
    </label>
    <button type="submit">Add Pet ✨</button>
  </form>
</section>
```

```
const mongoose = require("mongoose");
const express = require("express");
```

```
<main class="card-container">
  <% for (let i = 0; i < pets.length; i++) { %>
    <div class="pet-card">
      <div class="pet-emoji"><%= pets[i].emoji %></div>
      <h2><%= pets[i].name %></h2>
      <p>Age: <%= pets[i].age %></p>
      <% if (pets[i].adopted === true) { %>
        <p>Status: <span class="status available">Adopted 💖</span></p>
      <% } else { %>
        <p>Status: <span class="status adopted">Available</span></p>
      <% } %>
      <button>Delete</button>
    </div>
  <% } %>
</main>
```

```
app.use(express.static(__dirname + "/public"));
app.use(express.json());
app.set("view engine", "ejs");

app.use((req, res, next) => {
  console.log(req.method + req.path);
  next();
});
```

```
const form = document.querySelector("form");

form.addEventListener("submit", async (e) => {
  e.preventDefault();

  const petData = new FormData(form);
  const reqBody = Object.fromEntries(petData);

  const response = await fetch("/add/pet", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(reqBody)
  });

  const data = await response.json();
  window.location.href = "/";
});
```

```
app.post("/add/pet", async (req, res) => {
  const newPet = await new Pet({
    name: req.body.name,
    emoji: req.body.emoji,
    age: req.body.age,
    adopted: req.body.adopted === "on",
  }).save();
  res.json(newPet);
});
```

```
const petSchema = new mongoose.Schema(
  {
    name: String,
    emoji: String,
    age: Number,
    adopted: Boolean
  },
  { timestamps: true }
);
```

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Adopt a Pet 🏡🐾</title>
  <link rel="stylesheet" href="/pets.css">
  <script src="/petScript.js" defer></script>
</head>
<body>
  <header>
    <h1>🐾 Adopt a Pet 🐾</h1>
    <p>Give a furry friend a forever home!</p>
  </header>



</body>
</html>
```

```
const Pet = mongoose.model("Pet", petSchema, "Pets");
```

```
app.get("/", async (req, res) => {
  const pets = await Pet.find({}).sort({ createdAt: -1 });
  res.render("pets.ejs", { pets });
});
```

```
body {
  font-family: 'Comic Sans MS', cursive, sans-serif;
  background: #fff8f0;
  margin: 0;
  padding: 0;
  color: #444;
}

header {
  background: linear-gradient(to right, #ffe0e9, #fff0f5);
  padding: 2rem 1rem;
  border-bottom: 5px dotted #ffb3c1;
  text-align: center;
}

h1 {
  margin: 0;
  font-size: 2.7rem;
  color: #ff4081;
}

header p {
  font-size: 1.3rem;
  margin-top: 0.5rem;
}

.form-section {
  background: #fff0f6;
  border: 3px dashed #ffc8dd;
  margin: 2rem auto;
  width: 90%;
  max-width: 500px;
  border-radius: 20px;
  padding: 1.5rem;
  box-shadow: 0 6px 10px rgba(255, 182, 193, 0.2);
}

form {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}

input[type="text"],
input[type="number"] {
  padding: 0.6rem;
  font-size: 1rem;
  border-radius: 10px;
  border: 2px solid #ffc8dd;
}

label {
  font-size: 1rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

button {
  background-color: #ff8fab;
  color: white;
  border: none;
  border-radius: 12px;
  padding: 0.8rem;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background-color: #ff5c8a;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2rem;
  padding: 2rem;
}

.pet-card {
  background: #fff0f5;
  border: 3px dotted #ffa6c9;
  border-radius: 25px;
  width: 200px;
  padding: 1.5rem;
  box-shadow: 0 10px 16px rgba(255, 182, 193, 0.2);
  transition: transform 0.3s ease;
}

.pet-card:hover {
  transform: scale(1.05);
}

.pet-emoji {
  font-size: 4rem;
  margin-bottom: 0.5rem;
}

.status {
  font-weight: bold;
  padding: 0.2rem 0.4rem;
  border-radius: 8px;
}

.status.available {
  color: #2e7d32;
  background: #c8e6c9;
}

.status.adopted {
  color: #d32f2f;
  background: #ffcdd2;
}
```

```
const app = express();
```

```
async function startServer() {
  await mongoose.connect(
    "mongodb+srv://SE12:CSH2025@cluster0.u9yhg.mongodb.net/CSHpets?retryWrites=true&w=majority&appName=Cluster0"
  );

  app.listen(3000, () => {
    console.log(`Server running.`);
  });
}

startServer();
```

