# Parking-book
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Parking Slot Booking</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background-color: #f8f9fa;
    }
    h1, h2 {
      text-align: center;
    }
    .controls {
      text-align: center;
      margin-bottom: 20px;
    }
    .controls input {
      padding: 8px;
      margin: 0 10px;
    }
    .parking-lot {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
      gap: 15px;
      max-width: 600px;
      margin: 0 auto;
    }
    .slot {
      padding: 20px;
      text-align: center;
      border: 2px solid #333;
      border-radius: 10px;
      cursor: pointer;
      background-color: #d4edda;
      transition: 0.3s;
      position: relative;
    }
    .slot.booked {
      background-color: #f8d7da;
      cursor: not-allowed;
      color: #777;
    }
    .bookings-list {
      margin-top: 10px;
      font-size: 0.8em;
      color: #555;
    }
  </style>
</head>
<body>
  <h1>Parking Slot Booking</h1>
  <div class="controls">
    <label>Start Time: <input type="time" id="startTime" /></label>
    <label>End Time: <input type="time" id="endTime" /></label>
  </div>
  <div class="parking-lot" id="parkingLot"></div>

  <script>
    const totalSlots = 20;
    const parkingLot = document.getElementById('parkingLot');
    const startTimeInput = document.getElementById('startTime');
    const endTimeInput = document.getElementById('endTime');

    const bookings = {}; // { slotId: [{ start, end }] }

    function updateBookingsDisplay(slotElement, slotId) {
      let bookingText = '';
      if (bookings[slotId] && bookings[slotId].length > 0) {
        bookingText = bookings[slotId]
          .map(b => `${b.start} - ${b.end}`)
          .join('<br>');
      }
      let listDiv = slotElement.querySelector('.bookings-list');
      if (!listDiv) {
        listDiv = document.createElement('div');
        listDiv.classList.add('bookings-list');
        slotElement.appendChild(listDiv);
      }
      listDiv.innerHTML = bookingText;
    }

    for (let i = 1; i <= totalSlots; i++) {
      const slot = document.createElement('div');
      slot.classList.add('slot');
      slot.textContent = `Slot ${i}`;
      slot.dataset.id = i;

      const label = document.createElement('div');
      label.classList.add('bookings-list');
      slot.appendChild(label);

      slot.addEventListener('click', () => {
        const start = startTimeInput.value;
        const end = endTimeInput.value;

        if (!start || !end || start >= end) {
          alert('Please select a valid time range.');
          return;
        }

        const slotId = slot.dataset.id;
        bookings[slotId] = bookings[slotId] || [];

        // Check for overlap
        const conflict = bookings[slotId].some(b => 
          !(end <= b.start || start >= b.end)
        );

        if (conflict) {
          alert(`Slot ${slotId} is already booked for that time.`);
        } else {
          bookings[slotId].push({ start, end });
          updateBookingsDisplay(slot, slotId);
          alert(`Slot ${slotId} booked from ${start} to ${end}`);
        }
      });

      parkingLot.appendChild(slot);
    }
  </script>
</body>
</html>
