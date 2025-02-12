// TechnicalPro.jsx - Main Component
import { useState, useEffect } from "react";

export default function TechnicalPro() {
  const [search, setSearch] = useState("");
  const [clients, setClients] = useState(JSON.parse(localStorage.getItem("clients")) || []);
  const [offers, setOffers] = useState(JSON.parse(localStorage.getItem("offers")) || []);
  const [tasks, setTasks] = useState(JSON.parse(localStorage.getItem("tasks")) || []);

  useEffect(() => {
    localStorage.setItem("clients", JSON.stringify(clients));
    localStorage.setItem("offers", JSON.stringify(offers));
    localStorage.setItem("tasks", JSON.stringify(tasks));
  }, [clients, offers, tasks]);

  const addItem = (setter, items, label) => {
    const newItem = prompt(`Προσθήκη νέου ${label}:`);
    if (newItem) setter([...items, newItem]);
  };

  const editItem = (setter, items, index) => {
    const updatedItem = prompt("Επεξεργασία καταχώρησης:", items[index]);
    if (updatedItem) {
      const newItems = [...items];
      newItems[index] = updatedItem;
      setter(newItems);
    }
  };

  const deleteItem = (setter, items, index) => {
    if (window.confirm("Σίγουρα θέλετε να διαγράψετε αυτήν την καταχώρηση;")) {
      setter(items.filter((_, i) => i !== index));
    }
  };

  const data = [
    { category: "Πελάτες", description: "Διαχείριση πελατών, προσθήκη και επεξεργασία.", items: clients, setter: setClients },
    { category: "Προσφορές", description: "Καταγραφή προσφορών ανά πελάτη.", items: offers, setter: setOffers },
    { category: "Εργασίες", description: "Καταγραφή εκκρεμών και ολοκληρωμένων εργασιών.", items: tasks, setter: setTasks }
  ];

  return (
    <div className="p-4">
      <h1 className="text-2xl font-bold mb-4">Technical Pro - Διαχείριση Εργολαβιών</h1>
      <input 
        type="text" 
        placeholder="Αναζήτηση..." 
        className="w-full p-2 mb-4 border rounded"
        value={search}
        onChange={(e) => setSearch(e.target.value)}
      />
      {data.map((section, index) => (
        <button 
          key={index} 
          className="mb-4 p-2 bg-blue-500 text-white rounded mr-2"
          onClick={() => addItem(section.setter, section.items, section.category)}
        >
          Προσθήκη {section.category}
        </button>
      ))}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        {data.map((item, index) => (
          <div key={index} className="bg-white shadow rounded-lg p-4">
            <h2 className="text-xl font-semibold">{item.category}</h2>
            <p>{item.description}</p>
            <ul className="mt-2">
              {item.items.map((entry, i) => (
                <li key={i} className="text-sm text-gray-700 flex justify-between items-center">
                  {entry}
                  <div>
                    <button className="ml-2 text-blue-500" onClick={() => editItem(item.setter, item.items, i)}>✏️</button>
                    <button className="ml-2 text-red-500" onClick={() => deleteItem(item.setter, item.items, i)}>🗑️</button>
                  </div>
                </li>
              ))}
            </ul>
          </div>
        ))}
      </div>
    </div>
  );
}

// Vite Configuration for Vercel
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  base: "./",
  build: {
    outDir: "dist"
  },
  server: {
    port: 3000
  }
});

// index.html - Root HTML File
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Technical Pro</title>
  <script type="module" src="/main.jsx"></script>
</head>
<body>
  <div id="root"></div>
</body>
</html>

// main.jsx - Entry Point
import React from "react";
import ReactDOM from "react-dom/client";
import TechnicalPro from "./TechnicalPro";
import "./styles.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <TechnicalPro />
  </React.StrictMode>
);

// styles.css - Basic Styles
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
}

#root {
  max-width: 800px;
  width: 100%;
  background: white;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
}
# technical-pro3
