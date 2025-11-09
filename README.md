# Real-Time Collaborative Drawing Canvas

This project is a real-time collaborative drawing web application developed using **HTML**, **CSS**, **JavaScript**, and **Node.js**.  
It allows multiple users to draw together on a shared canvas, with all updates synchronized in real time through **native WebSocket communication**.  

The synchronization layer has been implemented manually, without using external frameworks like Socket.io, to provide a deeper understanding of the WebSocket protocol and real-time data handling.

---

## Setup Instructions

To set up and run the project locally:

### 1. Clone the Repository
git clone https://github.com/oorjithareddy/collaborative-canvas
cd collaborative-canvas
### 2. Install Dependencies

npm install
### 3. Start the Server
npm start
### 4. Open the Application
After running the server, open your browser and visit:
http://localhost:3000
### 5. Testing with Multiple Users
To test real-time collaboration:

Open http://localhost:3000 in multiple browser tabs or on different systems connected to the same network.

Draw in one tab and observe that changes instantly appear in the others.

Test Undo, Redo, and Clear functions to verify proper synchronization.

### Features
Real-time collaborative drawing across multiple clients

Adjustable brush color and stroke width

Eraser tool for selective clearing

Undo and redo operations shared across users

Clear entire canvas

Display of connected users and their cursor positions

Consistent synchronization using WebSockets

Known Limitations / Bugs
Undo and Redo are global, not per-user. Any user’s undo affects the shared canvas.

The canvas state is not persisted; all data is lost when the server restarts.

No authentication or user identification is currently implemented.

Minor latency differences may occur depending on the network connection.

How It Works
Each connected user communicates with the server via a WebSocket connection.

When a user draws, stroke data (coordinates, color, width) is sent to the server.

The server broadcasts this data to all other connected clients.

Each client updates its local canvas to maintain a synchronized state.

When a new user joins, the server sends the full existing canvas state to them.

Time Spent on the Project
Task	Duration
Canvas drawing and event handling	2 hours
WebSocket setup and testing	2 hours
Undo/Redo logic implementation	1.5 hours
Toolbar setup and configuration	1 hour
Testing and debugging	1.5 hours
Total Time Spent	8 hours

Folder Structure
pgsql
Copy code
collaborative-canvas/
│
├── client/
│   ├── index.html          → Main UI for canvas and toolbar
│   ├── style.css           → Styling and responsive layout
│   ├── main.js             → Initializes tools and event listeners
│   ├── canvas.js           → Handles drawing and rendering logic
│   └── websocket.js        → Manages client-side WebSocket communication
│
├── server/
│   ├── server.js           → WebSocket server using Node.js
│   ├── drawing-state.js    → Manages strokes and undo/redo logic
│   └── room.js             → Handles connected users and rooms
│
├── package.json
├── README.md
└── ARCHITECTURE.md
Future Enhancements
Add shape drawing tools (rectangles, circles, lines)

Add fill and text tools

Implement per-user undo/redo

Enable saving and restoring drawings from a database

Improve latency handling and smoothing for continuous drawing

Support multiple drawing rooms with isolated sessions

Author
Oorjitha Bhimavarapu
B.Tech Electronics and Computer Engineering
Amrita Vishwa Vidyapeetham, Bengaluru, India

This project provided practical experience with WebSocket-based synchronization, efficient canvas rendering, and multi-user real-time system design.
