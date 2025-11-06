# 🧩 Frontend Scenario-Based Interview Questions & Practice Guide

A collection of **20 practical frontend interview problems** — based on real-world scenarios that test your **React, JavaScript, localStorage, and API integration** skills.

---

## 🧠 1. Core JavaScript Logic & DOM Handling

### 🚗 1. Parking Area App
Manage car and bike parking spots. Record in-time and out-time, calculate parking charges (car ₹10/min, bike ₹5/min), and track total earnings per spot.  
Use **localStorage** for data persistence.

---

### ⏱️ 2. Stopwatch with History
Implement a stopwatch with **start, pause, reset** using `setInterval`.  
Save multiple time sessions, calculate total active time, and store data in **localStorage**.

---

### ⛽ 3. Fuel Station Management
Record vehicle type, liters filled, and rate per liter.  
Calculate total fuel sold and total revenue for the day.  
Data should persist in **localStorage**.

---

### 💧 4. Water Usage Tracker
Track tap on/off duration, calculate total water usage (`time × flow rate`), and maintain a consumption history.

---

### 📚 5. Library Management App
Allow adding, borrowing, and returning books.  
Track availability and borrowed count. Persist library data in **localStorage**.

---

## ⚛️ 2. React State Management & CRUD Functionality

### ✅ 6. To-Do App with Categories
Add, update, and delete tasks with categories like **Work, Personal, Urgent**.  
Filter tasks by completion status and category. Persist with **localStorage**.

---

### 💰 7. Expense Tracker
Add income and expenses, calculate total balance, filter by category/date, and display transaction history.  
Store all records in **localStorage**.

---

### 📦 8. Inventory Management System
Manage products with fields like **name, stock quantity, and price**.  
Add/remove items, calculate total inventory value, and persist data.

---

### 🏋️‍♂️ 9. Gym Membership Tracker
Track member check-in/check-out times.  
Display total active members and total time spent inside the gym.  
Store data locally.

---

### 👨‍💼 10. Employee Attendance Tracker
Each employee has check-in/out times.  
Compute daily working hours and total active time.  
Persist attendance logs in **localStorage**.

---

## 🌐 3. API Integration & Data Fetching

### 🛒 11. Product Cart System (FakeStore API)
Fetch products from the [FakeStore API](https://fakestoreapi.com).  
Allow users to add/remove items, adjust quantity, calculate total, and persist the cart.

---

### 🧾 12. Product Filter App
Fetch products and filter by **price range, category, or rating**.  
Display the total count of visible products dynamically.

---

### 🌦️ 13. Weather Dashboard
Fetch live weather data using a public API.  
Display **temperature, humidity, condition**, and store recent searches in localStorage.

---

### 🎟️ 14. Movie Ticket Booking App
Display seat layout, allow seat selection, and calculate total price by seat type (VIP ₹300, Normal ₹150).  
Show summary and store booking data.

---

### 🍔 15. Online Food Ordering Dashboard
Fetch menu items, calculate order total, and filter by **delivered/pending**.  
Store order history in **localStorage**.

---

## ⚙️ 4. LocalStorage & Data Persistence Focus

### 🧩 16. Quiz Application
Display one question at a time, store quiz progress and final score.  
Allow retry functionality using **localStorage**.

---

### 🗳️ 17. Online Polling App
Let users vote once per poll.  
Display total votes, percentages, and prevent multiple votes with **localStorage**.

---

### 🏨 18. Hotel Room Booking System
Record room bookings with check-in and check-out times.  
Calculate stay cost (₹ per night) and show total earnings.

---

### 🚌 19. Bus Seat Reservation System
Show a seat map. Allow booking and canceling seats.  
Calculate total booked/available seats and total revenue.  
Store booking data persistently.

---

### 🧑‍🏫 20. Classroom Attendance App
Mark students present/absent.  
Compute attendance percentage and store daily records for multiple days using **localStorage**.

---

## 📊 Suggested Practice Flow (Progressive Difficulty)

| Level | Focus | Scenarios | Example Projects |
|-------|--------|------------|------------------|
| 🟢 **Beginner** | JS + DOM | 1–5 | Parking, Stopwatch, Library |
| 🟡 **Intermediate** | React CRUD | 6–10 | To-Do, Expense Tracker |
| 🟠 **Upper-Intermediate** | API Fetching | 11–15 | Product Cart, Weather |
| 🔵 **Advanced** | localStorage + State Sync | 16–20 | Quiz, Polling, Hotel Booking |

---

## 💡 How to Use This for Practice

1. **Pick one scenario per day.**  
2. Sketch the UI layout and list all required state variables.  
3. Write core logic (JS or React hooks).  
4. Add persistence via `localStorage` or an API.  
5. Reflect on optimization (reusability, clean code, performance).

---

### 🚀 Pro Tip
You can turn these 20 problems into a **mini-project portfolio** — each one showcases real-world frontend skills employers love to see:
- State handling (`useState`, `useEffect`)
- Data persistence (`localStorage`)
- User interaction (event handling, timers)
- API usage (`fetch`, `async/await`)
- Reusability (components & modular structure)

---

**Author:** *Practice Set inspired by real Frontend Interview Scenarios*  
**Prepared for:** Frontend Developers mastering React + JavaScript fundamentals.  
**Version:** v1.0
