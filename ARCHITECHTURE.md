
---
# Architecture Documentation

This document describes the architecture, data flow, and design decisions behind the **Real-Time Collaborative Drawing Canvas** project.  
It focuses on how real-time drawing data is exchanged between clients and the server using WebSockets and how consistency is maintained across all connected users.

---

## 1. Flow Summary:  
Every drawing event originates from a user’s canvas, is transmitted to the server, and is broadcast to all other clients to maintain synchronization.

---

## 2. WebSocket Protocol

All messages exchanged between the client and server use JSON objects.  
Each message includes a `type` field and an optional `payload` field.

### Message Types

| Type | Direction | Description |
|------|------------|-------------|
| `DRAW` | Client → Server | Sends real-time stroke data as the user moves the cursor. |
| `STROKE` | Client → Server | Sends a completed stroke after the user stops drawing. |
| `SYNC_STATE` | Server → Client | Sends the entire canvas state for synchronization when a new user joins. |
| `UNDO` | Client ↔ Server | Performs a global undo operation across all users. |
| `REDO` | Client ↔ Server | Performs a global redo operation across all users. |
| `CLEAR` | Client ↔ Server | Clears the shared canvas for all connected users. |
| `CURSOR` | Client ↔ Server | Sends and updates cursor positions for all active users. |
| `PRESENCE` | Server → Client | Sends an updated list of connected users. |

---

## 3. Undo/Redo Strategy

The undo/redo functionality is implemented through the **DrawingState** module on the server.

- Each stroke is represented as an object containing:
  - `points`: List of coordinate pairs  
  - `color`: Stroke color  
  - `width`: Stroke thickness  
  - `active`: Boolean flag indicating visibility  

### Undo Operation
- The server marks the most recent active stroke as inactive.  
- The updated stroke list is broadcast to all connected clients.  

### Redo Operation
- The server reactivates the most recently undone stroke.  
- The new state is broadcast to all clients.  

By managing strokes instead of pixel data, synchronization remains efficient and lightweight.

---

## 4. Performance Decisions

Several optimizations were made to ensure smooth real-time performance:

1. **Local Rendering First:**  
   Drawing actions are first rendered on the local client for immediate visual feedback before synchronization with the server.

2. **Throttled WebSocket Messages:**  
   Messages are sent at controlled intervals (approximately 60 times per second) to avoid excessive network traffic.

3. **Minimal Data Exchange:**  
   Only essential stroke data (coordinates, color, and width) is transmitted, reducing bandwidth usage.

4. **Efficient Redrawing:**  
   The canvas can be reconstructed at any time from stored stroke data, enabling smooth resizing and reconnection recovery.

5. **No External Libraries:**  
   All communication uses the native WebSocket API to provide full control over performance and data flow.

---

## 5. Conflict Resolution

When multiple users draw simultaneously:

- Each stroke is recorded independently with its own timestamp.  
- The rendering sequence naturally determines the visual layering of strokes.  
- The **global undo/redo mechanism** ensures that all clients share the same canvas state after every operation.  
- The server acts as the single source of truth to prevent data conflicts between users.  

This approach ensures consistency without the need for complex conflict resolution logic.

---

## 6. Component Overview

### Client-Side
- **index.html:** Provides the layout for the canvas and toolbar.  
- **style.css:** Defines the user interface and responsive design.  
- **main.js:** Initializes UI tools, color picker, and connects event listeners.  
- **canvas.js:** Handles drawing actions, stroke storage, undo/redo, and resizing.  
- **websocket.js:** Manages real-time communication with the server.

### Server-Side
- **server.js:** Implements the WebSocket server using Node.js, managing connections and message routing.  
- **drawing-state.js:** Maintains in-memory storage for all strokes and handles undo/redo logic.  
- **room.js:** Manages connected users, rooms, and broadcasts updates.

---

## 7. Scalability Considerations

To make the system production-ready and scalable:

- Use a persistent storage system (MongoDB or Redis) for stroke data.  
- Implement room-based collaboration for multiple canvases.  
- Add compression and rate-limiting for WebSocket messages.  
- Introduce authentication for identifying users.  
- Employ load balancing across multiple WebSocket servers.

---

## 8. Limitations

- The application uses **in-memory storage**, so drawings are lost after a server restart.  
- Undo/redo operations apply to the **entire canvas** instead of individual users.  
- No **authentication** or user management system is included.  
- Designed primarily for **local or small-group use**, not large-scale deployment.  

---

## 9. Conclusion

This architecture demonstrates a lightweight and efficient approach to real-time collaboration using native WebSockets.  
By avoiding external frameworks, the design emphasizes transparency, performance, and conceptual clarity.  
The modular structure allows for future extensions such as persistent storage, multiple rooms, and user-specific operations.

---

## Author

**Shaik Adnan Tousef**  
B.Tech Computer Science Engineering
Amrita Vishwa Vidyapeetham, Bengaluru, India  

This project provided practical exposure to real-time WebSocket communication, data synchronization, and interactive canvas rendering.


