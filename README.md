# cookie.html
<!DOCTYPE html>
<html>
<head>
    <style>
        .voucher-box { border: 2px dashed #ee4d2d; padding: 15px; background: #fff5f1; border-radius: 8px; margin-top: 10px; }
        .btn-claim { background: #ee4d2d; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; }
        .btn-claim:hover { background: #d73211; }
        #log { font-family: monospace; font-size: 12px; margin-top: 15px; color: #333; }
    </style>
</head>
<body>

    <h3>Tool Lưu Mã Shopee Tự Động</h3>
    
    <p>1. Link mã giảm giá của bạn:</p>
    <input type="text" id="voucherUrl" style="width: 100%; padding: 8px;" 
           value="https://shopee.vn/voucher/details?evcode=RlNWLTEzNTQ4Mzg0Nzg5ODMxNzY%3D&from_source=voucher-wallet&promotionId=1354838478983176&signature=89a4baf57e4eb7dcbe8832795ad682597257c9f1e8af345d987ebd7f2122440d">

    <p>2. Dán Cookie của bạn:</p>
    <textarea id="myCookie" style="width: 100%; height: 80px;" placeholder="Dán SPC_ST vào đây..."></textarea>

    <div class="voucher-box">
        <button class="btn-claim" onclick="handleClaim()">BẮT ĐẦU LƯU MÃ NÀY</button>
        <div id="log"></div>
    </div>

    <script>
        async function handleClaim() {
            const url = document.getElementById('voucherUrl').value;
            const cookie = document.getElementById('myCookie').value;
            const logDiv = document.getElementById('log');

            if (!cookie) { alert("Thiếu Cookie rồi bạn ơi!"); return; }

            // Bước 1: Tách dữ liệu từ link bạn gửi
            const params = new URLSearchParams(new URL(url).search);
            const evcode = params.get('evcode');
            const promotionId = params.get('promotionId');
            const signature = params.get('signature');

            logDiv.innerHTML = `Đang xử lý PromotionID: ${promotionId}...`;

            // Bước 2: Gửi Request (Giả lập)
            try {
                const response = await fetch('https://shopee.vn/api/v2/voucher/claim', {
                    method: 'POST',
                    headers: {
                        'Cookie': cookie,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        voucher_code: evcode,
                        promotionid: parseInt(promotionId),
                        signature: signature
                    })
                });

                // Lưu ý: Kết quả thường sẽ bị chặn bởi CORS nếu chạy file html thuần
                logDiv.innerHTML += "<br>Kết quả: Đã gửi yêu cầu. (Nếu dùng Extension sẽ thành công)";
            } catch (err) {
                logDiv.innerHTML += "<br>Lỗi: " + err.message;
            }
        }
    </script>
</body>
</html>
