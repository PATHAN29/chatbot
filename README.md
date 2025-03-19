# chatbot
/* Frontend: React.js Medical Chatbot */

import React, { useState } from "react";
import axios from "axios";
import "./styles/App.css"; // Updated path to correctly locate App.css

const Chatbot = () => {
  const [message, setMessage] = useState("");
  const [responses, setResponses] = useState([]);

  const sendMessage = async () => {
    if (!message.trim()) return;
    const userMessage = { role: "user", content: message };
    setResponses([...responses, userMessage]);
    setMessage("");

    try {
      const res = await axios.post("http://localhost:5000/chat", { message });
      const botMessage = { role: "bot", content: res.data.reply };
      setResponses((prev) => [...prev, botMessage]);
    } catch (error) {
      console.error("Error fetching response", error);
    }
  };

  return (
    <div className="chat-container">
      <h2>Medical AI Chatbot</h2>
      <div className="chat-box">
        {responses.map((msg, index) => (
          <p key={index} className={msg.role === "user" ? "user" : "bot"}>
            {msg.role === "user" ? "You: " : "Bot: "}{msg.content}
          </p>
        ))}
      </div>
      <input
        type="text"
        value={message}
        onChange={(e) => setMessage(e.target.value)}
        placeholder="Type your message..."
      />
      <button onClick={sendMessage}>Send</button>
    </div>
  );
};

export default Chatbot;
