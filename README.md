[package.json](https://github.com/user-attachments/files/26038882/package.json)
{
  "name": "apex-auto-trade",
  "version": "1.0.0",
  "description": "Auto-trade backend connecting Tradovate, NinjaTrader, Project X, Webull, and Coinbase",
  "main": "server/index.js",
  "scripts": {
    "start":   "node server/index.js",
    "dev":     "nodemon server/index.js",
    "test":    "node server/test-connections.js"
  },
  "dependencies": {
    "axios":      "^1.6.0",
    "cors":       "^2.8.5",
    "dotenv":     "^16.3.0",
    "express":    "^4.18.0",
    "jsonwebtoken": "^9.0.0",
    "ws":         "^8.16.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.0"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
