<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Receipt Maker - Mapetla Car Wash</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      margin: 0;
      padding: 0;
      background: linear-gradient(to bottom right, #1a3b6d, #4a7bb7);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: #333;
    }
    .container {
      max-width: 500px;
      width: 100%;
      margin: 20px;
      padding: 30px;
      background: white;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
      border-radius: 12px;
    }
    h1 {
      text-align: center;
      color: #1a3b6d;
      margin-bottom: 25px;
      font-size: 2em;
      text-transform: uppercase;
      letter-spacing: 2px;
    }
    label {
      display: block;
      margin: 10px 0 5px;
      font-weight: bold;
      color: #1a3b6d;
    }
    input, textarea, button {
      width: 100%;
      padding: 12px;
      margin: 5px 0 15px;
      border: 1px solid #4a7bb7;
      border-radius: 6px;
      box-sizing: border-box;
    }
    button {
      background: linear-gradient(to right, #1a3b6d, #4a7bb7);
      color: white;
      font-size: 16px;
      cursor: pointer;
      border: none;
      transition: transform 0.3s ease;
    }
    button:hover {
      transform: scale(1.05);
      opacity: 0.9;
    }
    #receipt-preview {
      display: none;
      max-width: 400px;
      padding: 30px;
      background: white;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
      border-radius: 12px;
      text-align: center;
    }
    #receipt-preview h2 {
      color: #1a3b6d;
      margin-bottom: 20px;
      border-bottom: 2px solid #4a7bb7;
      padding-bottom: 10px;
    }
    #receipt-preview p {
      margin: 10px 0;
      text-align: left;
    }
    .company-details {
      font-size: 0.8em;
      color: #666;
      text-align: center;
      margin-bottom: 15px;
      line-height: 1.5;
    }
    footer {
      text-align: center;
      margin-top: 20px;
      color: rgba(255,255,255,0.7);
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Mapetla Car Wash</h1>
    <form id="receipt-form">
      <label for="cashier">Cashier Name:</label>
      <input type="text" id="cashier" placeholder="Enter cashier's name" required>

      <label for="name">Customer Name:</label>
      <input type="text" id="name" placeholder="Enter customer's name" required>

      <label for="date">Date:</label>
      <input type="date" id="date" required>

      <label for="receiptNo">Receipt Number:</label>
      <input type="text" id="receiptNo" readonly>

      <label for="items">Items/Services:</label>
      <textarea id="items" placeholder="e.g., Car Wash: R100, Vacuuming: R50" required></textarea>

      <label for="total">Total Amount (R):</label>
      <input type="number" id="total" placeholder="e.g., 150" required>

      <button type="button" id="generate">Generate Receipt</button>
    </form>
  </div>

  <!-- Receipt Preview Section -->
  <div id="receipt-preview">
    <h2>Mapetla Car Wash</h2>
    <div class="company-details">
      <p>959 Makhetha Street, Phiritona<br>
      Heilbron, 9650<br>
      Tel: 083 435 9005</p>
    </div>
    <p id="preview-cashier" style="font-weight: bold;"></p>
    <p id="preview-date"></p>
    <p id="preview-receiptNo"></p>
    <p id="preview-name"></p>
    <p><strong>Items/Services:</strong></p>
    <p id="preview-items"></p>
    <p id="preview-total" style="font-weight: bold;"></p>
    <p style="text-align: center; font-style: italic;">Thank you for choosing Mapetla Car Wash!</p>
    <p id="preview-timestamp" style="font-size: 0.8em; color: #666;"></p>
  </div>

  <footer>
    &copy; 2024 Mapetla Car Wash. All rights reserved.
  </footer>

  <script>
    // Function to generate a unique receipt number
    function generateReceiptNumber() {
      const now = new Date();
      const year = now.getFullYear().toString().slice(-2);
      const month = String(now.getMonth() + 1).padStart(2, '0');
      const day = String(now.getDate()).padStart(2, '0');
      
      // Generate a 4-digit random number
      const randomPart = String(Math.floor(1000 + Math.random() * 9000));
      
      // Combine parts: YYMMDD-XXXX format
      return `${year}${month}${day}-${randomPart}`;
    }

    // Set a default receipt number when the page loads
    window.addEventListener('load', () => {
      document.getElementById('receiptNo').value = generateReceiptNumber();
    });

    document.getElementById('generate').addEventListener('click', () => {
      // Get form values
      const cashier = document.getElementById('cashier').value.trim();
      const name = document.getElementById('name').value.trim();
      const date = document.getElementById('date').value;
      const receiptNo = document.getElementById('receiptNo').value.trim();
      const items = document.getElementById('items').value.trim();
      const total = document.getElementById('total').value.trim();

      // Validate inputs
      if (!cashier || !name || !date || !items || !total) {
        alert("Please fill in all the fields.");
        return;
      }

      // Get current date and time
      const now = new Date();
      const formattedDateTime = now.toLocaleString('en-ZA', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit',
        second: '2-digit'
      });

      // Fill receipt preview
      document.getElementById('preview-cashier').innerText = `Cashier: ${cashier}`;
      document.getElementById('preview-date').innerText = `Date: ${date}`;
      document.getElementById('preview-receiptNo').innerText = `Receipt No: ${receiptNo}`;
      document.getElementById('preview-name').innerText = `Customer: ${name}`;
      document.getElementById('preview-items').innerText = items;
      document.getElementById('preview-total').innerText = `Total Amount: R${total}`;
      document.getElementById('preview-timestamp').innerText = `Receipt Generated: ${formattedDateTime}`;

      // Show receipt preview
      const receiptPreview = document.getElementById('receipt-preview');
      receiptPreview.style.display = 'block';

      // Generate JPG and download
      html2canvas(receiptPreview).then(canvas => {
        const imgData = canvas.toDataURL("image/jpeg");
        const link = document.createElement("a");
        link.href = imgData;
        link.download = `Receipt_${receiptNo}.jpg`;
        link.click();
      }).catch(error => {
        console.error("Error generating JPG:", error);
        alert("An error occurred while generating the JPG. Check the console for details.");
      });

      // Generate a new receipt number for the next receipt
      document.getElementById('receiptNo').value = generateReceiptNumber();
    });
  </script>
</body>
</html>
