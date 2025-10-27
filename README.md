const mongoose = require("mongoose");

//  Connect to MongoDB
mongoose.connect("mongodb://127.0.0.1:27017/productDB", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
.then(() => console.log(" MongoDB Connected"))
.catch(err => console.error("no", err));

//  Define Schema & Model
const productSchema = new mongoose.Schema({
  name: String,
  price: Number,
  category: String
});
const Product = mongoose.model("Product", productSchema);

//  CRUD Operations
const run = async () => {
  await Product.deleteMany(); // clear old data

  // CREATE
  const p1 = await Product.create({ name: "Laptop", price: 1000, category: "Electronics" });
  const p2 = await Product.create({ name: "Shoes", price: 80, category: "Fashion" });
  console.log(" Created:", p1, p2);

  // READ
  console.log("\n📖 All Products:", await Product.find());
  console.log("\n📖 Filtered (Electronics):", await Product.find({ category: "Electronics" }));

  // UPDATE
  const updated = await Product.findOneAndUpdate({ name: "Laptop" }, { price: 1200 }, { new: true });
  console.log("\n Updated:", updated);

  // DELETE
  const deleted = await Product.findOneAndDelete({ name: "Shoes" });
  console.log("\n Deleted:", deleted);

  console.log("\n📖 Final Products:", await Product.find());
  mongoose.connection.close();
};

run()
