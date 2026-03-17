<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Romantic Depot | 成人用品超级商场</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .product-card:hover { transform: translateY(-5px); transition: all 0.3s ease; }
        .cart-badge { position: absolute; top: -8px; right: -8px; background: #ef4444; color: white; border-radius: 50%; width: 20px; height: 20px; display: flex; align-items: center; justify-content: center; font-size: 12px; }
        .chat-window { position: fixed; bottom: 80px; right: 20px; width: 350px; height: 500px; background: white; border-radius: 12px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); display: none; flex-direction: column; z-index: 1000; }
        .chat-window.open { display: flex; }
    </style>
</head>
<body class="bg-gray-50 text-gray-900">
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;

        const App = () => {
            const [products, setProducts] = useState([]);
            const [cart, setCart] = useState([]);
            const [view, setView] = useState('home'); // home, cart, checkout, about, policies, locations
            const [showChat, setShowChat] = useState(false);
            const [messages, setMessages] = useState([]);
            const [inputMessage, setInputMessage] = useState('');
            const [userInfo, setUserInfo] = useState({ name: '', address: '', phone: '' });
            const [paymentAddress] = useState('TBBFsXpjB8MP38KXhU7D1RijTd8GsKfFyX');

            useEffect(() => {
                fetch('data/products.json')
                    .then(res => res.json())
                    .then(data => setProducts(data));
            }, []);

            const addToCart = (product) => {
                setCart([...cart, product]);
            };

            const removeFromCart = (index) => {
                const newCart = [...cart];
                newCart.splice(index, 1);
                setCart(newCart);
            };

            const totalPrice = cart.reduce((sum, item) => sum + item.price, 0);

            const handleCheckout = (e) => {
                e.preventDefault();
                if (!userInfo.name || !userInfo.address || !userInfo.phone) {
                    alert('请填写完整的收货信息');
                    return;
                }
                
                // Mock order submission
                const order = {
                    id: Date.now(),
                    customer: userInfo,
                    items: cart,
                    total: totalPrice,
                    status: 'pending',
                    timestamp: new Date().toISOString()
                };
                
                // In a real app, this would be an API call
                const orders = JSON.parse(localStorage.getItem('orders') || '[]');
                orders.push(order);
                localStorage.setItem('orders', JSON.stringify(orders));
                
                setView('payment');
            };

            const sendMessage = (e) => {
                e.preventDefault();
                if (!inputMessage.trim()) return;
                
                const newMessage = { text: inputMessage, sender: 'user', time: new Date().toLocaleTimeString() };
                const newMessages = [...messages, newMessage];
                setMessages(newMessages);
                setInputMessage('');
                
                // Mock auto-reply
                setTimeout(() => {
                    setMessages(prev => [...prev, { text: "客服正在处理您的请求，请稍候...", sender: 'admin', time: new Date().toLocaleTimeString() }]);
                }, 1000);

                // Store in localStorage for admin panel
                const chatHistory = JSON.parse(localStorage.getItem('chatHistory') || '[]');
                chatHistory.push(newMessage);
                localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
            };

            const Nav = () => (
                <nav className="bg-white shadow-sm sticky top-0 z-50">
                    <div className="max-container mx-auto px-4 py-4 flex justify-between items-center">
                        <h1 className="text-2xl font-bold text-rose-600 cursor-pointer" onClick={() => setView('home')}>Romantic Depot</h1>
                        <div className="flex gap-6 items-center">
                            <button onClick={() => setView('home')} className="hover:text-rose-600 transition">商城</button>
                            <button onClick={() => setView('about')} className="hover:text-rose-600 transition">关于我们</button>
                            <button onClick={() => setView('policies')} className="hover:text-rose-600 transition">政策</button>
                            <button onClick={() => setView('locations')} className="hover:text-rose-600 transition">地理位置</button>
                            <div className="relative cursor-pointer" onClick={() => setView('cart')}>
                                <i className="fas fa-shopping-cart text-xl"></i>
                                {cart.length > 0 && <span className="cart-badge">{cart.length}</span>}
                            </div>
                        </div>
                    </div>
                </nav>
            );

            const ProductList = () => (
                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8 py-8 px-4">
                    {products.map(product => (
                        <div key={product.id} className="bg-white rounded-xl shadow-sm overflow-hidden product-card border border-gray-100">
                            <div className="h-64 overflow-hidden">
                                <img src={product.image} alt={product.name} className="w-full h-full object-cover" />
                            </div>
                            <div className="p-4">
                                <h3 className="font-semibold text-lg mb-2 h-14 overflow-hidden">{product.name}</h3>
                                <div className="flex justify-between items-end">
                                    <div>
                                        <p className="text-gray-400 line-through text-sm">MSRP: ${product.msrp.toFixed(2)}</p>
                                        <p className="text-rose-600 font-bold text-xl">${product.price.toFixed(2)}</p>
                                    </div>
                                    <button 
                                        onClick={() => addToCart(product)}
                                        className="bg-rose-600 text-white px-4 py-2 rounded-lg hover:bg-rose-700 transition"
                                    >
                                        加入购物车
                                    </button>
                                </div>
                            </div>
                        </div>
                    ))}
                </div>
            );

            const CartView = () => (
                <div className="max-w-4xl mx-auto py-12 px-4">
                    <h2 className="text-3xl font-bold mb-8 text-center">您的购物车</h2>
                    {cart.length === 0 ? (
                        <div className="text-center py-20 bg-white rounded-xl shadow-sm">
                            <i className="fas fa-shopping-basket text-6xl text-gray-200 mb-4"></i>
                            <p className="text-gray-500">购物车是空的</p>
                            <button onClick={() => setView('home')} className="mt-4 text-rose-600 font-semibold underline">去购物</button>
                        </div>
                    ) : (
                        <div className="bg-white rounded-xl shadow-sm p-6">
                            {cart.map((item, index) => (
                                <div key={index} className="flex items-center gap-4 py-4 border-b last:border-0">
                                    <img src={item.image} className="w-20 h-20 object-cover rounded-lg" />
                                    <div className="flex-1">
                                        <h4 className="font-semibold">{item.name}</h4>
                                        <p className="text-rose-600 font-bold">${item.price.toFixed(2)}</p>
                                    </div>
                                    <button onClick={() => removeFromCart(index)} className="text-gray-400 hover:text-red-500">
                                        <i className="fas fa-trash"></i>
                                    </button>
                                </div>
                            ))}
                            <div className="mt-8 pt-8 border-t flex justify-between items-center">
                                <div className="text-2xl font-bold">总计: <span className="text-rose-600">${totalPrice.toFixed(2)}</span></div>
                                <button 
                                    onClick={() => setView('checkout')}
                                    className="bg-rose-600 text-white px-8 py-3 rounded-xl font-bold hover:bg-rose-700 transition"
                                >
                                    去结算
                                </button>
                            </div>
                        </div>
                    )}
                </div>
            );

            const CheckoutView = () => (
                <div className="max-w-2xl mx-auto py-12 px-4">
                    <h2 className="text-3xl font-bold mb-8 text-center">填写收货信息</h2>
                    <form onSubmit={handleCheckout} className="bg-white rounded-xl shadow-sm p-8 space-y-6 border border-gray-100">
                        <div>
                            <label className="block text-sm font-semibold mb-2">姓名</label>
                            <input 
                                type="text" required
                                value={userInfo.name}
                                onChange={(e) => setUserInfo({...userInfo, name: e.target.value})}
                                className="w-full px-4 py-3 rounded-lg border focus:ring-2 focus:ring-rose-500 outline-none transition"
                                placeholder="请输入您的姓名"
                            />
                        </div>
                        <div>
                            <label className="block text-sm font-semibold mb-2">详细地址</label>
                            <textarea 
                                required
                                value={userInfo.address}
                                onChange={(e) => setUserInfo({...userInfo, address: e.target.value})}
                                className="w-full px-4 py-3 rounded-lg border focus:ring-2 focus:ring-rose-500 outline-none transition"
                                rows="3"
                                placeholder="请输入您的详细收货地址"
                            ></textarea>
                        </div>
                        <div>
                            <label className="block text-sm font-semibold mb-2">电话号码</label>
                            <input 
                                type="tel" required
                                value={userInfo.phone}
                                onChange={(e) => setUserInfo({...userInfo, phone: e.target.value})}
                                className="w-full px-4 py-3 rounded-lg border focus:ring-2 focus:ring-rose-500 outline-none transition"
                                placeholder="请输入您的联系电话"
                            />
                        </div>
                        <div className="pt-4">
                            <button 
                                type="submit"
                                className="w-full bg-rose-600 text-white py-4 rounded-xl font-bold text-lg hover:bg-rose-700 transition"
                            >
                                确认信息并支付
                            </button>
                        </div>
                    </form>
                </div>
            );

            const PaymentView = () => (
                <div className="max-w-2xl mx-auto py-12 px-4 text-center">
                    <div className="bg-white rounded-xl shadow-sm p-12 border border-gray-100">
                        <i className="fas fa-check-circle text-6xl text-green-500 mb-6"></i>
                        <h2 className="text-3xl font-bold mb-4">订单已提交</h2>
                        <p className="text-gray-600 mb-8 text-lg">请转账至以下收款地址完成支付：</p>
                        <div className="bg-gray-50 p-6 rounded-xl border-2 border-dashed border-gray-200 break-all font-mono text-xl select-all">
                            {paymentAddress}
                        </div>
                        <p className="mt-8 text-sm text-gray-400">支付完成后，我们将尽快为您发货</p>
                        <button 
                            onClick={() => setView('home')}
                            className="mt-8 text-rose-600 font-semibold"
                        >
                            返回首页
                        </button>
                    </div>
                </div>
            );

            const AboutView = () => (
                <div className="max-w-4xl mx-auto py-12 px-4">
                    <h2 className="text-3xl font-bold mb-8">关于我们</h2>
                    <div className="bg-white rounded-xl shadow-sm p-8 prose prose-rose max-w-none">
                        <p className="text-lg text-gray-700 leading-relaxed">
                            Romantic Depot 是成人用品超级商场，库存超过 100,000 件商品。
                            我们为您的浪漫需求提供广泛的产品。我们的商店为所有客户提供安全、舒适的环境。
                        </p>
                        <img src="https://romanticdepot.com/wp-content/uploads/2021/11/romantic-depot-store.jpg" className="w-full rounded-xl my-8" />
                        <p className="text-gray-600">
                            我们的使命是为客户提供最优质的成人用品，并在整个购物过程中保持绝对的隐私和自由。
                        </p>
                    </div>
                </div>
            );

            const PoliciesView = () => (
                <div className="max-w-4xl mx-auto py-12 px-4">
                    <h2 className="text-3xl font-bold mb-8">政策说明</h2>
                    <div className="grid md:grid-cols-2 gap-8">
                        <div className="bg-white rounded-xl shadow-sm p-8">
                            <h3 className="text-xl font-bold mb-4 flex items-center gap-2">
                                <i className="fas fa-truck text-rose-600"></i> 配送政策
                            </h3>
                            <p className="text-gray-600">
                                我们对所有订单提供谨慎的配送服务。大多数订单在 24-48 小时内处理。包裹上不会显示任何敏感信息。
                            </p>
                        </div>
                        <div className="bg-white rounded-xl shadow-sm p-8">
                            <h3 className="text-xl font-bold mb-4 flex items-center gap-2">
                                <i className="fas fa-undo text-rose-600"></i> 退换货政策
                            </h3>
                            <p className="text-gray-600">
                                由于产品的特殊性质，我们不接受已使用商品的退货。如果有质量问题，请联系客服处理。
                            </p>
                        </div>
                        <div className="bg-white rounded-xl shadow-sm p-8 md:col-span-2">
                            <h3 className="text-xl font-bold mb-4 flex items-center gap-2">
                                <i className="fas fa-user-secret text-rose-600"></i> 隐私保护
                            </h3>
                            <p className="text-gray-600">
                                您的隐私是我们的首要任务。所有交易记录和运输标签都是完全保密的。我们承诺绝不向第三方泄露您的任何个人信息。
                            </p>
                        </div>
                    </div>
                </div>
            );

            const LocationsView = () => (
                <div className="max-w-4xl mx-auto py-12 px-4">
                    <h2 className="text-3xl font-bold mb-8">地理位置</h2>
                    <div className="bg-white rounded-xl shadow-sm p-8">
                        <div className="flex flex-col md:flex-row gap-8 items-center">
                            <div className="flex-1">
                                <h3 className="text-2xl font-bold mb-4">纽约旗舰店</h3>
                                <div className="space-y-4 text-gray-600">
                                    <p className="flex items-center gap-3">
                                        <i className="fas fa-map-marker-alt text-rose-600"></i>
                                        123 Romantic Way, New York, NY 10001
                                    </p>
                                    <p className="flex items-center gap-3">
                                        <i className="fas fa-phone text-rose-600"></i>
                                        (212) 555-0199
                                    </p>
                                    <p className="flex items-center gap-3">
                                        <i className="fas fa-clock text-rose-600"></i>
                                        周一至周日: 10:00 AM - 11:00 PM
                                    </p>
                                </div>
                            </div>
                            <div className="w-full md:w-1/2 h-64 bg-gray-100 rounded-xl overflow-hidden">
                                <img src="https://images.unsplash.com/photo-1517733948473-28b9a11a0930?auto=format&fit=crop&q=80&w=800" className="w-full h-full object-cover" />
                            </div>
                        </div>
                    </div>
                </div>
            );

            return (
                <div className="min-h-screen">
                    <Nav />
                    
                    <main className="max-w-7xl mx-auto">
                        {view === 'home' && <ProductList />}
                        {view === 'cart' && <CartView />}
                        {view === 'checkout' && <CheckoutView />}
                        {view === 'payment' && <PaymentView />}
                        {view === 'about' && <AboutView />}
                        {view === 'policies' && <PoliciesView />}
                        {view === 'locations' && <LocationsView />}
                    </main>

                    {/* Chat Toggle Button */}
                    <button 
                        onClick={() => setShowChat(!showChat)}
                        className="fixed bottom-6 right-6 w-14 h-14 bg-rose-600 text-white rounded-full shadow-lg flex items-center justify-center text-2xl z-50 hover:bg-rose-700 transition"
                    >
                        <i className={`fas ${showChat ? 'fa-times' : 'fa-comment-dots'}`}></i>
                    </button>

                    {/* Chat Window */}
                    <div className={`chat-window ${showChat ? 'open' : ''}`}>
                        <div className="bg-rose-600 p-4 rounded-t-12 text-white font-bold flex justify-between items-center">
                            <span>在线客服</span>
                            <i className="fas fa-minus cursor-pointer" onClick={() => setShowChat(false)}></i>
                        </div>
                        <div className="flex-1 overflow-y-auto p-4 space-y-4 bg-gray-50">
                            {messages.length === 0 && (
                                <div className="text-center text-gray-400 mt-10">
                                    <p>欢迎来到 Romantic Depot</p>
                                    <p className="text-sm">有什么可以帮您的？</p>
                                </div>
                            )}
                            {messages.map((msg, i) => (
                                <div key={i} className={`flex ${msg.sender === 'user' ? 'justify-end' : 'justify-start'}`}>
                                    <div className={`max-w-[80%] p-3 rounded-2xl text-sm ${
                                        msg.sender === 'user' 
                                        ? 'bg-rose-600 text-white rounded-tr-none' 
                                        : 'bg-white text-gray-800 shadow-sm rounded-tl-none'
                                    }`}>
                                        {msg.text}
                                        <div className="text-[10px] mt-1 opacity-50">{msg.time}</div>
                                    </div>
                                </div>
                            ))}
                        </div>
                        <form onSubmit={sendMessage} className="p-4 bg-white border-t flex gap-2">
                            <input 
                                type="text" 
                                value={inputMessage}
                                onChange={(e) => setInputMessage(e.target.value)}
                                className="flex-1 px-4 py-2 bg-gray-100 rounded-full outline-none focus:ring-1 focus:ring-rose-500 text-sm"
                                placeholder="输入消息..."
                            />
                            <button type="submit" className="text-rose-600 text-xl px-2">
                                <i className="fas fa-paper-plane"></i>
                            </button>
                        </form>
                    </div>

                    <footer className="bg-gray-900 text-white py-12 mt-20">
                        <div className="max-w-7xl mx-auto px-4 grid grid-cols-1 md:grid-cols-4 gap-8">
                            <div>
                                <h4 className="text-xl font-bold mb-4">Romantic Depot</h4>
                                <p className="text-gray-400 text-sm">您的私人浪漫生活顾问</p>
                            </div>
                            <div>
                                <h4 className="font-semibold mb-4">快速链接</h4>
                                <ul className="space-y-2 text-gray-400 text-sm">
                                    <li><button onClick={() => setView('home')}>首页</button></li>
                                    <li><button onClick={() => setView('about')}>关于我们</button></li>
                                    <li><button onClick={() => setView('policies')}>政策</button></li>
                                </ul>
                            </div>
                            <div>
                                <h4 className="font-semibold mb-4">客户服务</h4>
                                <ul className="space-y-2 text-gray-400 text-sm">
                                    <li>联系我们</li>
                                    <li>常见问题</li>
                                    <li>配送信息</li>
                                </ul>
                            </div>
                            <div>
                                <h4 className="font-semibold mb-4">联系方式</h4>
                                <p className="text-gray-400 text-sm">Email: support@romanticdepot.com</p>
                                <p className="text-gray-400 text-sm">Tel: (212) 555-0199</p>
                            </div>
                        </div>
                        <div className="text-center mt-12 pt-8 border-t border-gray-800 text-gray-500 text-xs">
                            © 2026 Romantic Depot. All Rights Reserved.
                        </div>
                    </footer>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
