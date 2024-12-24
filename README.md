# eazybytes
Project: Stock Trading Simulation System
import React, { useState, useEffect } from 'react';

interface Stock {
  id: number;
  name: string;
  price: number;
  quantity: number;
}

interface Portfolio {
  id: number;
  stock: Stock;
  quantity: number;
}

const StockTradingSimulationSystem = () => {
  const [stocks, setStocks] = useState<Stock[]>([
    { id: 1, name: 'Apple', price: 150.0, quantity: 0 },
    { id: 2, name: 'Google', price: 2500.0, quantity: 0 },
    { id: 3, name: 'Amazon', price: 3000.0, quantity: 0 },
  ]);

  const [portfolio, setPortfolio] = useState<Portfolio[]>([]);
  const [balance, setBalance] = useState<number>(10000.0);

  const handleBuy = (stock: Stock) => {
    if (balance >= stock.price) {
      setBalance(balance - stock.price);
      const existingStock = portfolio.find((p) => p.stock.id === stock.id);
      if (existingStock) {
        existingStock.quantity += 1;
        setPortfolio([...portfolio.filter((p) => p.stock.id !== stock.id), existingStock]);
      } else {
        setPortfolio([...portfolio, { id: portfolio.length + 1, stock, quantity: 1 }]);
      }
    }
  };

  const handleSell = (stock: Stock) => {
    const existingStock = portfolio.find((p) => p.stock.id === stock.id);
    if (existingStock && existingStock.quantity > 0) {
      setBalance(balance + stock.price);
      existingStock.quantity -= 1;
      setPortfolio([...portfolio.filter((p) => p.stock.id !== stock.id), existingStock]);
    }
  };

  return (
    <div className="max-w-7xl mx-auto p-4">
      <h1 className="text-3xl font-bold mb-4">Stock Trading Simulation System</h1>
      <div className="flex flex-wrap justify-center mb-4">
        {stocks.map((stock) => (
          <div key={stock.id} className="bg-white rounded-lg shadow-md p-4 w-1/3 mx-2 my-2">
            <h2 className="text-lg font-bold mb-2">{stock.name}</h2>
            <p className="text-lg mb-2">Price: ${stock.price}</p>
            <button
              className="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
              onClick={() => handleBuy(stock)}
            >
              Buy
            </button>
            <button
              className="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded ml-2"
              onClick={() => handleSell(stock)}
            >
              Sell
            </button>
          </div>
        ))}
      </div>
      <h2 className="text-2xl font-bold mb-4">Portfolio</h2>
      <div className="flex flex-wrap justify-center mb-4">
        {portfolio.map((p) => (
          <div key={p.id} className="bg-white rounded-lg shadow-md p-4 w-1/3 mx-2 my-2">
            <h2 className="text-lg font-bold mb-2">{p.stock.name}</h2>
            <p className="text-lg mb-2">Quantity: {p.quantity}</p>
          </div>
        ))}
      </div>
      <h2 className="text-2xl font-bold mb-4">Balance: ${balance}</h2>
    </div>
  );
};

export default StockTradingSimulationSystem;
