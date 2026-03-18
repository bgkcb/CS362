# **STEP 1**
---
เลือก Feature Booking + seatlock

### **1. Customer**

Attributes: userId,email

### **2. Event**

Attributes: eventId,eventName,eventDate

### **3.Seat**

Attributes: seatId,zone,seatStatus (available, locked, booked),lockTimestamp

### **4.Booking**

Attributes: bookingId,numberOfTickets,bookingStatus (pending, confirmed, cancelled)

### **5. Payment**

Attributes: paymentId,amount,paymentStatus

### **6. Ticket**

Attributes: ticketId,qrcode

---

#### Customer 1 --- 0..* Booking
→ ลูกค้าหนึ่งคนสามารถมีหลายการจอง

#### Booking 1 --- 1..* Seat
→ การจองหนึ่งครั้งสามารถมีหลายที่นั่ง

#### Event 1 --- 1..* Seat
→ อีเวนต์หนึ่งมีหลายที่นั่ง

#### Booking 1 --- 1 Payment
→ การจองหนึ่งครั้งมีการชำระเงินหนึ่งครั้ง

#### Booking 1 --- 1..* Ticket
→ การจองหนึ่งครั้งสามารถสร้างหลาย Ticket

---

# **Step 3**

---

## **Task 1: Controller Contract (API Specification)**

สำหรับ core feature Booking Ticket for Event

#### **1) Create Booking**

**Endpoint:** /bookings

**Method:** POST

**Request JSON:**
{
  "userId": "U001",
  "eventId": "E001",
  "seatIds": ["S001", "S002"],
  "numberOfTickets": 2
}

**Response JSON:**
{
  "bookingId": "B001",
  "bookingStatus": "pending",
  "numberOfTickets": 2,
  "message": "Booking created successfully"
}

#### **2) Get Booking by ID**

**Endpoint:** /bookings/{bookingId}

**Method:** GET

**Response JSON:**
{
  "bookingId": "B001",
  "userId": "U001",
  "eventId": "E001",
  "seatIds": ["S001", "S002"],
  "numberOfTickets": 2,
  "bookingStatus": "pending"
}

#### **3) Cancel Booking**

**Endpoint:** /bookings/{bookingId}

**Method:** DELETE

**Response JSON:**
{
  "message": "Booking cancelled successfully"
}

#### **4) Get Seats of Event**

**Endpoint:** /events/{eventId}/seats

**Method:** GET

**Response JSON:**

[
  {
    "seatId": "S001",
    "zone": "A",
    "seatStatus": "available"
  },
  {
    "seatId": "S002",
    "zone": "A",
    "seatStatus": "booked"
  }
]

---
## **Task 2**
type BookingService interface {

    // สำหรับ POST /bookings
    CreateBooking(req BookingRequest) (BookingResponse, error)
    
    // สำหรับ GET /bookings/{bookingId}
    GetBookingByID(bookingID string) (BookingDetails, error)

    // สำหรับ DELETE /bookings/{bookingId}
    CancelBooking(bookingID string) (MessageResponse, error)

    // สำหรับ GET /events/{eventId}/seats
    GetSeatsByEvent(eventID string) ([]SeatStatus, error)
  
}

---
## **Task 3**

1. Entity: Booking (ข้อมูลการจอง)
   isValid(): ตรวจสอบว่าข้อมูลพื้นฐานที่จำเป็นครบถ้วนหรือไม่ (เช่น userId, eventId, และ seatIds ต้องไม่เป็นค่าว่าง) ก่อนจะส่งไปประมวลผลต่อ
   calculateTotal(pricePerSeat: double): คำนวณราคาสุทธิโดยนำจำนวนที่นั่งที่เลือก (seatIds.length) คูณกับราคาต่อที่นั่งที่ส่งเข้ามา
   updateStatus(newStatus: String): จัดการการเปลี่ยนสถานะของ Object การจอง เช่น เปลี่ยนจาก pending เป็น confirmed หรือ cancelled เพื่อให้ State ของข้อมูลถูกต้องเสมอ
