<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Shopping List App</title>
  <script src="https://cdn.jsdelivr.net/npm/react@18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/react-dom@18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@babel/standalone@7.20.15/babel.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
    // Static Data (Expanded)
    const categories = [
      {
        name: "Vegetables & Fruits",
        icon: "🥦",
        items: [
          // Vegetables
          "Carrot", "Potato", "Sweet Potato", "Beet", "Radish", "Turnip", "Parsnip", "Rutabaga", "Yam", "Cassava",
          "Daikon", "Celeriac", "Salsify", "Horseradish", "Jicama", "Spinach", "Kale", "Lettuce", "Swiss Chard",
          "Arugula", "Collard Greens", "Mustard Greens", "Bok Choy", "Cabbage", "Watercress", "Endive", "Romaine",
          "Mizuna", "Tatsoi", "Broccoli", "Cauliflower", "Brussels Sprouts", "Kohlrabi", "Romanesco", "Broccolini",
          "Chinese Broccoli", "Onion", "Garlic", "Shallot", "Leek", "Green Onion", "Chive", "Ramp", "Tomato",
          "Cucumber", "Zucchini", "Eggplant", "Bell Pepper", "Chili Pepper", "Pumpkin", "Butternut Squash", "Acorn Squash",
          "Okra", "Avocado", "Corn", "Peas", "Green Beans", "Snap Peas", "Snow Peas", "Edamame", "Fava Beans", "Lentils",
          "Chickpeas", "Asparagus", "Celery", "Rhubarb", "Bamboo Shoot", "Fennel", "Artichoke", "Cardoon", "Nopales",
          "Seaweed", "Water Chestnut", "Udupi Mattu Gulla Eggplant",
          // Fruits
          "Apple", "Banana", "Orange", "Strawberry", "Mango", "Watermelon", "Blueberry", "Pineapple", "Grape", "Lemon",
          "Lime", "Peach", "Pear", "Cherry", "Plum", "Kiwi", "Raspberry", "Blackberry", "Apricot", "Papaya", "Pomegranate",
          "Fig", "Grapefruit", "Tangerine", "Nectarine", "Dragon Fruit", "Passion Fruit", "Guava", "Lychee", "Rambutan",
          "Mangosteen", "Durian", "Jackfruit", "Starfruit", "Persimmon", "Soursop", "Custard Apple", "Tamarind", "Acai",
          "Breadfruit", "Longan", "Pomelo", "Salak", "Feijoa", "Cranberry", "Gooseberry", "Elderberry", "Currant",
          "Huckleberry", "Bilberry", "Cloudberry", "Cantaloupe", "Honeydew", "Galia Melon", "Horned Melon", "Clementine",
          "Mandarin", "Kumquat", "Yuzu", "Blood Orange", "Ugli Fruit", "Date", "Olive", "Coconut", "Prune", "Loquat",
          "Medlar", "Quince", "Pluot", "Greengage", "Rose Apple", "Surinam Cherry", "Ackee", "Jabuticaba", "Camu Camu", "Noni"
        ]
      },
      {
        name: "Grocery Basics",
        icon: "🛒",
        items: ["Rice", "Pasta", "Flour", "Sugar", "Salt", "Pepper", "Olive Oil", "Vegetable Oil", "Ketchup", "Soy Sauce",
                "Vinegar", "Quinoa", "Barley", "Oats", "Miso", "Tamari", "Mustard", "Honey", "Maple Syrup"]
      },
      {
        name: "Dairy & Eggs",
        icon: "🥛",
        items: ["Milk", "Cheese", "Yogurt", "Cream", "Butter", "Eggs", "Cheddar", "Mozzarella", "Greek Yogurt", "Duck Eggs"]
      },
      {
        name: "Meat & Poultry",
        icon: "🥩",
        items: ["Chicken Breast", "Whole Chicken", "Beef", "Lamb", "Fish", "Pork", "Turkey", "Goat", "Duck"]
      },
      {
        name: "Drinks & Snacks",
        icon: "🧃",
        items: ["Water", "Orange Juice", "Apple Juice", "Soda", "Chips", "Cookies", "Chocolate", "Popcorn", "Nuts", "Granola Bars"]
      },
      {
        name: "Household Essentials",
        icon: "🏡",
        items: ["Detergent", "Dish Soap", "Shampoo", "Soap", "Toothpaste", "Toilet Paper", "Paper Towels", "Bleach", "Hand Sanitizer"]
      },
      {
        name: "Kids' Essentials",
        icon: "🧸",
        items: ["Diapers", "Baby Food", "Crayons", "Juice Box", "Kids Snacks", "School Supplies"]
      },
      {
        name: "Medicine Basics",
        icon: "💊",
        items: ["Pain Reliever", "Bandages", "Antiseptic", "Cough Syrup", "Vitamins"]
      },
      {
        name: "Recipes",
        icon: "🍴",
        items: [
          { name: "Chicken Soup", ingredients: ["Chicken", "Carrots", "Onion", "Salt"] },
          { name: "Salad Bowl", ingredients: ["Lettuce", "Tomato", "Cucumber", "Olive Oil"] },
          { name: "Chocolate Cookies", ingredients: ["Flour", "Sugar", "Chocolate", "Butter"] },
          { name: "Simple Pasta", ingredients: ["Pasta", "Tomato Sauce", "Cheese", "Salt"] }
        ]
      },
      {
        name: "Special Occasion Lists",
        icon: "🎁",
        items: ["Eid Essentials", "Ramadan Essentials", "Birthday Party"]
      }
    ];

    // App Context for Cart
    const CartContext = React.createContext();

    function App() {
      const [cart, setCart] = React.useState([]);
      const [search, setSearch] = React.useState("");

      const addToCart = (item, quantity) => {
        setCart([...cart, { item, quantity }]);
      };

      const removeFromCart = (index) => {
        setCart(cart.filter((_, i) => i !== index));
      };

      // Memoized search results
      const searchResults = React.useMemo(() => {
        if (!search) return [];
        const results = [];
        categories.forEach(category => {
          if (category.name !== "Recipes" && category.name !== "Special Occasion Lists") {
            category.items.forEach(item => {
              if (typeof item === "string" && item.toLowerCase().includes(search.toLowerCase())) {
                results.push({ item, category: category.name });
              }
            });
          }
        });
        return results;
      }, [search]);

      return (
        <CartContext.Provider value={{ cart, addToCart, removeFromCart }}>
          <div className="min-h-screen bg-[#FFF8E7] text-[#2F2F2F]">
            {window.location.hash === "#/cart" ? (
              <CartPage />
            ) : window.location.hash.startsWith("#/category/") ? (
              <CategoryPage />
            ) : (
              <HomePage search={search} setSearch={setSearch} searchResults={searchResults} />
            )}
          </div>
        </CartContext.Provider>
      );
    }

    function HomePage({ search, setSearch, searchResults }) {
      const { addToCart } = React.useContext(CartContext);
      const [quantities, setQuantities] = React.useState({});

      const handleQuantityChange = (item, value) => {
        setQuantities({ ...quantities, [item]: value });
      };

      return (
        <div>
          <header className="bg-[#F4A261] text-[#2F2F2F] p-4">
            <h1 className="text-2xl font-bold">Shopping List App</h1>
            <p className="mt-2">Plan Your Shopping with Ease!</p>
            <input
              type="text"
              placeholder="Search items (e.g., Milk, Tomato)"
              value={search}
              onChange={(e) => setSearch(e.target.value)}
              className="mt-2 p-2 w-full max-w-md rounded bg-[#FDFCFA] text-[#2F2F2F] border border-[#E9C46A]"
            />
          </header>
          <main className="p-4">
            {search && searchResults.length > 0 ? (
              <div className="space-y-2">
                <h2 className="text-xl font-semibold">Search Results</h2>
                {searchResults.map((result, index) => (
                  <div key={index} className="bg-[#FDFCFA] p-4 rounded shadow flex items-center justify-between">
                    <span>{result.item} ({result.category})</span>
                    <div className="flex items-center">
                      <select
                        value={quantities[result.item] || 1}
                        onChange={(e) => handleQuantityChange(result.item, parseInt(e.target.value))}
                        className="p-1 border border-[#E9C46A] rounded mr-2 bg-[#FDFCFA]"
                      >
                        {[1, 2, 3, 4, 5, 6, 7, 8, 9, 10].map(n => (
                          <option key={n} value={n}>{n}x</option>
                        ))}
                      </select>
                      <button
                        onClick={() => addToCart(result.item, quantities[result.item] || 1)}
                        className="bg-[#E9C46A] text-[#2F2F2F] px-4 py-2 rounded hover:bg-[#F4A261]"
                      >
                        Add to List
                      </button>
                    </div>
                  </div>
                ))}
              </div>
            ) : search && searchResults.length === 0 ? (
              <p>No items found.</p>
            ) : (
              <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
                {categories.map((category) => (
                  <a
                    key={category.name}
                    href={`#/category/${category.name}`}
                    className="bg-[#FDFCFA] p-4 rounded shadow hover:shadow-lg cursor-pointer flex items-center"
                  >
                    <span className="text-2xl mr-2">{category.icon}</span>
                    <h2 className="text-lg font-semibold">{category.name}</h2>
                  </a>
                ))}
              </div>
            )}
          </main>
        </div>
      );
    }

    function CategoryPage() {
      const { addToCart } = React.useContext(CartContext);
      const categoryName = decodeURIComponent(window.location.hash.replace("#/category/", ""));
      const category = categories.find(c => c.name === categoryName);
      const [quantities, setQuantities] = React.useState({});

      if (!category) return <div>Category not found</div>;

      const handleQuantityChange = (item, value) => {
        setQuantities({ ...quantities, [item]: value });
      };

      const addItem = (item) => {
        const quantity = quantities[item] || 1;
        addToCart(item, quantity);
        setQuantities({ ...quantities, [item]: 1 });
      };

      return (
        <div className="p-4">
          <a href="#" className="text-[#F4A261] underline mb-4 inline-block">← Back to Home</a>
          <h1 className="text-2xl font-bold mb-4">{category.icon} {category.name}</h1>
          {category.name === "Recipes" ? (
            <div className="space-y-4">
              {category.items.map((recipe) => (
                <div key={recipe.name} className="bg-[#FDFCFA] p-4 rounded shadow">
                  <h2 className="text-lg font-semibold">{recipe.name}</h2>
                  <ul className="list-disc pl-5">
                    {recipe.ingredients.map((ing) => (
                      <li key={ing}>{ing}</li>
                    ))}
                  </ul>
                  <button
                    onClick={() => recipe.ingredients.forEach(ing => addToCart(ing, 1))}
                    className="mt-2 bg-[#E9C46A] text-[#2F2F2F] px-4 py-2 rounded hover:bg-[#F4A261]"
                  >
                    Add All Ingredients to List
                  </button>
                </div>
              ))}
            </div>
          ) : (
            <div className="space-y-2">
              {category.items.map((item) => (
                <div key={typeof item === "string" ? item : item.name} className="bg-[#FDFCFA] p-4 rounded shadow flex items-center justify-between">
                  <span>{typeof item === "string" ? item : item.name}</span>
                  <div className="flex items-center">
                    <select
                      value={quantities[item] || 1}
                      onChange={(e) => handleQuantityChange(item, parseInt(e.target.value))}
                      className="p-1 border border-[#E9C46A] rounded mr-2 bg-[#FDFCFA]"
                    >
                      {[1, 2, 3, 4, 5, 6, 7, 8, 9, 10].map(n => (
                        <option key={n} value={n}>{n}x</option>
                      ))}
                    </select>
                    <button
                      onClick={() => addItem(item)}
                      className="bg-[#E9C46A] text-[#2F2F2F] px-4 py-2 rounded hover:bg-[#F4A261]"
                    >
                      Add to List
                    </button>
                  </div>
                </div>
              ))}
            </div>
          )}
        </div>
      );
    }

    function CartPage() {
      const { cart, removeFromCart } = React.useContext(CartContext);

      const saveAsImage = () => {
        const cartElement = document.getElementById("cart-content");
        html2canvas(cartElement).then(canvas => {
          const link = document.createElement("a");
          link.download = "shopping-list.png";
          link.href = canvas.toDataURL("image/png");
          link.click();
        });
      };

      return (
        <div className="p-4">
          <a href="#" className="text-[#F4A261] underline mb-4 inline-block">← Back to Home</a>
          <h1 className="text-2xl font-bold mb-4">🛒 Your Shopping List</h1>
          <div id="cart-content" className="bg-[#FDFCFA] p-4 rounded shadow">
            {cart.length === 0 ? (
              <p>Your list is empty.</p>
            ) : (
              <ul className="space-y-2">
                {cart.map((entry, index) => (
                  <li key={index} className="flex justify-between items-center">
                    <span>{entry.item} ({entry.quantity}x)</span>
                    <button
                      onClick={() => removeFromCart(index)}
                      className="text-[#F4A261] hover:underline"
                    >
                      Remove
                    </button>
                  </li>
                ))}
              </ul>
            )}
          </div>
          {cart.length > 0 && (
            <div className="mt-4 flex space-x-4">
              <button
                onClick={() => alert("Viewing list on screen")}
                className="bg-[#E9C46A] text-[#2F2F2F] px-4 py-2 rounded hover:bg-[#F4A261]"
              >
                View List
              </button>
              <button
                onClick={saveAsImage}
                className="bg-[#E9C46A] text-[#2F2F2F] px-4 py-2 rounded hover:bg-[#F4A261]"
              >
                Save as Image
              </button>
            </div>
          )}
        </div>
      );
    }

    ReactDOM.render(<App />, document.getElementById("root"));
  </script>
</body>
</html>
