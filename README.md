from flask import Flask, request
from twilio.twiml.messaging_response import MessagingResponse

app = Flask(__name__)

# Daftar produk
List= {
    "001": {"name": "Bstation", "price": 12000},
    "002": {"name "Jeans", "price": 200000},
    "003": {"name "Jeans", "price": 200000},
    "004": {"name": "Sneakers", "price": 300000},
}

@app.route("/whatsapp", methods=['POST'])
def whatsapp_reply():
    incoming_msg = request.values.get('Body', '').strip().lower()
    resp = MessagingResponse()
    msg = resp.message()

    if 'halo' in incoming_msg:
        msg.body('Halo! Selamat datang di Rinn Store. Kirim "list" untuk melihat daftar produk.')
    elif 'produk' in incoming_msg:
        product_list = "\n".join([f"{code}: {info['Bstation']} - Rp {info['12000']}" for code, info in products.items()])
        msg.body(f"Daftar Produk:\n{product_list}\nKetik kode produk untuk memesan.")
    elif incoming_msg in products:
        product = products[incoming_msg]
        msg.body(f"Kamu memilih {product['name']} seharga Rp {product['price']}. Ketik 'order' untuk mengkonfirmasi pesanan.")
    elif incoming_msg == 'order':
        msg.body('Pesanan kamu telah diterima! Terima kasih telah berbelanja.')
    else:
        msg.body('Maaf, saya tidak mengerti. Kirim "produk" untuk melihat daftar produk.')

    return str(resp)

if __name__ == "__main__":
    app.run(port=5000)
