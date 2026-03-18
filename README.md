STEP 1
เลือก Feature Booking + seatlock

1. Customer
Attributes: userId,email
2. Event
Attributes: eventId,eventName,eventDate
3.Seat
Attributes: seatId,zone,seatStatus (available, locked, booked),lockTimestamp
4.Booking
Attributes:bookingId,numberOfTickets,bookingStatus (pending, confirmed, cancelled)
5. Payment
Attributes:paymentId,amount,paymentStatus
6. Ticket
Attributes:ticketId,qrcode

Customer 1 --- 0..* Booking
→ ลูกค้าหนึ่งคนสามารถมีหลายการจอง

Booking 1 --- 1..* Seat
→ การจองหนึ่งครั้งสามารถมีหลายที่นั่ง

Event 1 --- 1..* Seat
→ อีเวนต์หนึ่งมีหลายที่นั่ง

Booking 1 --- 1 Payment
→ การจองหนึ่งครั้งมีการชำระเงินหนึ่งครั้ง

Booking 1 --- 1..* Ticket
→ การจองหนึ่งครั้งสามารถสร้างหลาย Ticket
