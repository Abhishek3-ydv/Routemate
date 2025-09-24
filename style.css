// --- MOCK DATA ---
const busData = [
    { id: 1, number: 'DHN-01', route: 'Sindri (BIT) to Dhanbad Station', status: 'yellow', delay: 4, crowd: 'medium', eta: 12, stops: ['BIT Sindri', 'Saharpura', 'Baliapur', 'Joraphatak', 'Dhanbad Bus Stand', 'Dhanbad Station'] },
    { id: 2, number: 'BOK-05', route: 'Chas to Jharia', status: 'green', delay: 0, crowd: 'low', eta: 8, stops: ['Chas More', 'Bokaro Steel City', 'Joraphatak', 'Dhansar', 'Jharia'] },
    { id: 3, number: 'SND-LCL', route: 'Sindri Local (Domgarh Loop)', status: 'red', delay: 9, crowd: 'high', eta: 18, stops: ['Domgarh', 'Rohrabandh', 'Manohartand', 'Chasnala', 'Sudamdih'] },
    { id: 4, number: 'DHN-CC', route: 'Dhanbad City Circle', status: 'green', delay: 0, crowd: 'low', eta: 25, stops: ['Bank More', 'City Center', 'ISM Gate', 'Bartand', 'Police Line'] }
];

const translations = {
    en: {
        // Headings
        liveStatus: 'Live Bus Status',
        bookTicket: 'Book Your Ticket',
        profileSettings: 'Profile & Settings',
        language: 'Language',
        mySubscription: 'My Subscription',
        // Form & Buttons
        from: 'From',
        to: 'To',
        passengers: 'Passengers',
        bookAndPay: 'Book & Pay',
        // Nav
        home: 'Home',
        ticket: 'Ticket',
        profile: 'Profile',
        // Bus Status
        onTime: 'On Time',
        delay: 'Delay',
        late: 'Late',
        seatsAvailable: 'Seats Available',
        halfFull: 'Half-full',
        likelyFull: 'Likely Full',
        eta: 'ETA',
        crowd: 'Crowd',
        share: 'Share',
        // Other
        searchPlaceholder: 'Search for bus or route...',
        alternateRoute: 'Alternate Route Suggested',
        route: 'Route',
        stops: 'Stops (Offline Map)',
        rateThisBus: 'Rate this Bus',
        excellent: 'Excellent',
        average: 'Average',
        poor: 'Poor',
        getTripSummary: 'Get Trip Summary'
    },
    hi: {
        // Headings
        liveStatus: 'लाइव बस स्थिति', bookTicket: 'अपनी टिकट बुक करें', profileSettings: 'प्रोफ़ाइल और सेटिंग्स', language: 'भाषा', mySubscription: 'मेरी सदस्यता',
        // Form & Buttons
        from: 'से', to: 'तक', passengers: 'यात्री', bookAndPay: 'बुक करें और भुगतान करें',
        // Nav
        home: 'होम', ticket: 'टिकट', profile: 'प्रोफ़ाइल',
        // Bus Status
        onTime: 'समय पर', delay: 'देरी', late: 'देर', seatsAvailable: 'सीटें उपलब्ध हैं', halfFull: 'आधा भरा हुआ', likelyFull: 'संभवतः भरा हुआ', eta: 'ETA', crowd: 'भीड़', share: 'शेयर करें',
        // Other
        searchPlaceholder: 'बस या मार्ग खोजें...', alternateRoute: 'वैकल्पिक मार्ग सुझाया गया', route: 'मार्ग', stops: 'स्टॉप (ऑफ़लाइन मानचित्र)', rateThisBus: 'इस बस को रेट करें', excellent: 'उत्कृष्ट', average: 'औसत', poor: 'खराब', getTripSummary: 'यात्रा सारांश प्राप्त करें'
    }
};

// --- GLOBAL STATE & ELEMENTS ---
let currentLanguage = 'en';
const pages = {
    home: document.getElementById('home-page'),
    ticket: document.getElementById('ticket-page'),
    profile: document.getElementById('profile-page'),
};
const navButtons = document.querySelectorAll('.nav-btn');
const busList = document.getElementById('bus-list');
const routeModal = document.getElementById('route-modal');
const modalContent = document.getElementById('modal-content');
const toast = document.getElementById('toast');
const bookingForm = document.getElementById('booking-form');
const fareEl = document.getElementById('fare');
const passengersInput = document.getElementById('passengers');

// --- INITIALIZATION ---
document.addEventListener('DOMContentLoaded', () => {
    renderBusList(busData);
    setupEventListeners();
    loadCachedData();
});

// --- DATA & CACHING ---
function loadCachedData() {
    // Simulate loading last known location from cache
    const cachedLocation = localStorage.getItem('lastLocation');
    if (cachedLocation) {
        // showToast(`Resuming from ${cachedLocation}`, 2500); // Removed this line
    } else {
         // Simulate finding location for the first time
         localStorage.setItem('lastLocation', 'Central Park');
    }
}

// --- UI RENDERING ---
function renderBusList(buses) {
    busList.innerHTML = '';
    if (buses.length === 0) {
        busList.innerHTML = `<p class="text-gray-500 text-center">No buses found.</p>`;
        return;
    }
    buses.forEach(bus => {
        const statusInfo = getStatusInfo(bus.status, bus.delay);
        const crowdInfo = getCrowdInfo(bus.crowd);
        const busCard = `
            <div class="bg-white p-4 rounded-lg shadow cursor-pointer hover:shadow-lg transition-shadow" onclick="openRouteModal(${bus.id})">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="font-bold text-lg">${bus.number}</p>
                        <p class="text-sm text-gray-600">${bus.route}</p>
                    </div>
                    <div class="${statusInfo.class} px-3 py-1 text-xs font-bold rounded-full">${statusInfo.text}</div>
                </div>
                <div class="mt-4 flex justify-between items-center text-sm">
                    <div class="text-center">
                        <p class="font-semibold">${getPredictiveETA(bus.eta)} min</p>
                        <p class="text-xs text-gray-500">${translations[currentLanguage].eta}</p>
                    </div>
                    <div class="text-center">
                        <p class="font-semibold ${crowdInfo.class}">${crowdInfo.text}</p>
                        <p class="text-xs text-gray-500">${translations[currentLanguage].crowd}</p>
                    </div>
                     <button onclick="event.stopPropagation(); shareLocation(${bus.id})" class="bg-blue-500 text-white px-4 py-2 rounded-full hover:bg-blue-600">
                         <i class="fa-solid fa-share-alt mr-2"></i><span>${translations[currentLanguage].share}</span>
                     </button>
                </div>
            </div>
        `;
        busList.innerHTML += busCard;
    });
}

function getStatusInfo(status, delay) {
    const lang = translations[currentLanguage];
    switch (status) {
        case 'green': return { text: lang.onTime, class: 'status-green' };
        case 'yellow': return { text: `${lang.delay} ${delay}m`, class: 'status-yellow' };
        case 'red': return { text: `${lang.late} ${delay}m`, class: 'status-red' };
        default: return { text: 'Unknown', class: 'bg-gray-400 text-white' };
    }
}

function getCrowdInfo(crowd) {
    const lang = translations[currentLanguage];
     switch (crowd) {
        case 'low': return { text: lang.seatsAvailable, class: 'crowd-low' };
        case 'medium': return { text: lang.halfFull, class: 'crowd-medium' };
        case 'high': return { text: lang.likelyFull, class: 'crowd-high' };
        default: return { text: 'N/A', class: 'text-gray-500' };
    }
}

function getPredictiveETA(baseEta) {
    // Simulate AI/ML based ETA prediction by adding a small random factor
    // E.g., Historical data suggests this route is often 2 mins slower
    const predictiveAdjustment = 2; 
    return baseEta + predictiveAdjustment;
}

// --- NAVIGATION ---
function navigateTo(pageKey) {
    Object.values(pages).forEach(page => page.classList.add('hidden'));
    if(pages[pageKey]) {
        pages[pageKey].classList.remove('hidden');
    }

    navButtons.forEach(btn => {
        if (btn.dataset.page === pageKey) {
            btn.classList.add('text-indigo-600');
            btn.classList.remove('text-gray-500');
        } else {
            btn.classList.remove('text-indigo-600');
            btn.classList.add('text-gray-500');
        }
    });
    window.scrollTo(0, 0); // Scroll to top on page change
}

// --- MODAL & TOAST ---
function openRouteModal(busId) {
    const bus = busData.find(b => b.id === busId);
    if (!bus) return;
    const lang = translations[currentLanguage];
    
    let stopsHtml = bus.stops.map((stop, index) => `
        <div class="flex items-center">
            <div class="flex flex-col items-center mr-4">
                <div class="w-4 h-4 rounded-full ${index === 0 ? 'bg-green-500' : 'bg-gray-300'}"></div>
                <div class="w-0.5 h-8 ${index === bus.stops.length - 1 ? 'bg-transparent' : 'bg-gray-300'}"></div>
            </div>
            <p class="font-medium">${stop}</p>
        </div>
    `).join('');

    modalContent.innerHTML = `
        <div class="p-5 border-b">
            <div class="flex justify-between items-center">
                <h3 class="text-xl font-bold">${lang.route}: ${bus.number}</h3>
                <button onclick="closeRouteModal()" class="text-gray-500 hover:text-gray-800 text-2xl leading-none">&times;</button>
            </div>
            <p class="text-sm text-gray-600">${bus.route}</p>
        </div>
        <div class="p-5">
            <h4 class="font-semibold mb-2">${lang.stops}</h4>
            ${stopsHtml}
        </div>
        <div class="p-5 border-t" id="gemini-summary-container">
            <button id="gemini-summary-btn" onclick="getTripSummary(${bus.id})" class="w-full bg-indigo-100 text-indigo-700 font-bold py-2 px-4 rounded-full hover:bg-indigo-200 transition-colors flex items-center justify-center">
               <span class="mr-2">✨</span> ${lang.getTripSummary}
            </button>
            <div id="gemini-summary-result" class="mt-3 text-sm text-gray-700"></div>
        </div>
        <div class="p-5 bg-gray-50 border-t">
            <h4 class="font-semibold mb-3">${lang.rateThisBus}</h4>
            <div class="flex justify-around items-center">
                 <button class="flex flex-col items-center text-gray-600 hover:text-green-500" onclick="submitFeedback(5)">
                     <i class="fa-solid fa-face-smile text-3xl mb-1"></i>
                     <span class="text-xs">${lang.excellent}</span>
                 </button>
                 <button class="flex flex-col items-center text-gray-600 hover:text-yellow-500" onclick="submitFeedback(3)">
                     <i class="fa-solid fa-face-meh text-3xl mb-1"></i>
                      <span class="text-xs">${lang.average}</span>
                 </button>
                 <button class="flex flex-col items-center text-gray-600 hover:text-red-500" onclick="submitFeedback(1)">
                     <i class="fa-solid fa-face-frown text-3xl mb-1"></i>
                     <span class="text-xs">${lang.poor}</span>
                 </button>
            </div>
        </div>
    `;
    routeModal.classList.remove('hidden');
}

function closeRouteModal() {
    routeModal.classList.add('hidden');
}

function submitFeedback(rating) {
    showToast(`Thank you for your feedback! (${rating} stars)`);
    setTimeout(closeRouteModal, 500);
}

function showToast(message, duration = 2000) {
    toast.textContent = message;
    toast.classList.remove('hidden');
    setTimeout(() => {
        toast.classList.add('hidden');
    }, duration);
}

// --- EVENT LISTENERS & HANDLERS ---
function setupEventListeners() {
    navButtons.forEach(button => {
        button.addEventListener('click', () => {
            navigateTo(button.dataset.page);
        });
    });

    routeModal.addEventListener('click', (e) => {
        if (e.target === routeModal) closeRouteModal();
    });
    
    document.getElementById('route-search').addEventListener('input', handleSearch);

    passengersInput.addEventListener('input', () => {
        const count = parseInt(passengersInput.value) || 1;
        fareEl.textContent = count * 20; // Assuming base fare is 20
    });
    
    bookingForm.addEventListener('submit', (e) => {
        e.preventDefault();
        showToast('Ticket booked successfully!', 3000);
        navigateTo('home');
    });
    
}

function handleSearch(e) {
    const searchTerm = e.target.value.toLowerCase();
    const recommendationCard = document.getElementById('route-recommendation');
    
    if (searchTerm.includes('101') || searchTerm.includes('mall')) {
        recommendationCard.classList.remove('hidden');
    } else {
        recommendationCard.classList.add('hidden');
    }

    const filteredBuses = busData.filter(bus => 
        bus.number.toLowerCase().includes(searchTerm) || 
        bus.route.toLowerCase().includes(searchTerm)
    );
    renderBusList(filteredBuses);
}

function shareLocation(busId) {
    const bus = busData.find(b => b.id === busId);
    const message = `Track my bus: Bus ${bus.number} will arrive at your stop in approx. ${getPredictiveETA(bus.eta)} mins. Live location: [simulated_map_link]`;
    const whatsappUrl = `https://api.whatsapp.com/send?text=${encodeURIComponent(message)}`;
    
    showToast('Share link copied to clipboard!');
    try {
        const tempInput = document.createElement('textarea');
        tempInput.value = message;
        document.body.appendChild(tempInput);
        tempInput.select();
        document.execCommand('copy');
        document.body.removeChild(tempInput);
    } catch (err) {
         console.error('Could not copy text: ', err);
    }
}

function applyTranslations() {
    const lang = translations[currentLanguage];
    // Helper to set text content
    const setText = (id, text) => {
        const el = document.getElementById(id);
        if (el) el.innerText = text;
    };

    // Update static text across pages
    setText('live-status-heading', lang.liveStatus);
    setText('alternate-route-heading', lang.alternateRoute);
    setText('book-ticket-heading', lang.bookTicket);
    setText('from-label', lang.from);
    setText('to-label', lang.to);
    setText('passengers-label', lang.passengers);
    setText('book-pay-btn', lang.bookAndPay);
    setText('profile-settings-heading', lang.profileSettings);
    setText('language-heading', lang.language);
    setText('subscription-heading', lang.mySubscription);
    
    // Update nav
    setText('nav-home', lang.home);
    setText('nav-ticket', lang.ticket);
    setText('nav-profile', lang.profile);
    
    // Update placeholder
    const searchInput = document.getElementById('route-search');
    if (searchInput) searchInput.placeholder = lang.searchPlaceholder;
}

function changeLanguage(lang) {
    if (!translations[lang]) return;
    currentLanguage = lang;

    const enBtn = document.getElementById('lang-btn-en');
    const hiBtn = document.getElementById('lang-btn-hi');
    if (lang === 'en') {
        enBtn.classList.add('bg-indigo-500', 'text-white');
        enBtn.classList.remove('bg-white', 'border', 'border-gray-300');
        hiBtn.classList.remove('bg-indigo-500', 'text-white');
        hiBtn.classList.add('bg-white', 'border', 'border-gray-300');
    } else {
        hiBtn.classList.add('bg-indigo-500', 'text-white');
        hiBtn.classList.remove('bg-white', 'border', 'border-gray-300');
        enBtn.classList.remove('bg-indigo-500', 'text-white');
        enBtn.classList.add('bg-white', 'border', 'border-gray-300');
    }
    
    applyTranslations();
    renderBusList(busData); // Re-render the dynamic bus list
    document.getElementById('language-btn').innerText = lang.toUpperCase();
}

// --- GEMINI API INTEGRATION ---
async function generateGeminiContent(prompt, targetElement, buttonElement) {
    if (buttonElement) buttonElement.disabled = true;
    targetElement.classList.remove('hidden');
    targetElement.innerHTML = `<div class="flex items-center justify-center p-4"><div class="animate-spin rounded-full h-5 w-5 border-b-2 border-indigo-500"></div><p class="ml-2 text-gray-500">Generating...</p></div>`;

    const apiKey = ""; // Per instructions, API key is handled by the environment
    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

    const payload = { contents: [{ parts: [{ text: prompt }] }] };

    try {
        let response;
        let delay = 1000;
        // Basic exponential backoff for retries
        for (let i = 0; i < 4; i++) {
            response = await fetch(apiUrl, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(payload) });
            if (response.ok) break;
            if (response.status === 429 || response.status >= 500) {
                await new Promise(resolve => setTimeout(resolve, delay));
                delay *= 2;
            } else break;
        }

        if (!response.ok) throw new Error(`API Error: ${response.status}`);

        const result = await response.json();
        const candidate = result.candidates?.[0];

        if (candidate && candidate.content?.parts?.[0]?.text) {
            let text = candidate.content.parts[0].text;
            // Simple markdown to HTML
            text = text.replace(/^\* (.*$)/gm, '<li class="ml-4 list-disc">$1</li>');
            text = text.replace(/\n/g, '<br>');
            targetElement.innerHTML = `<div class="p-2">${text}</div>`;
        } else {
            throw new Error("Invalid response from API.");
        }
    } catch (error) {
        console.error("Gemini API Error:", error);
        targetElement.innerHTML = `<p class="text-red-500 text-center">Sorry, couldn't generate a response. Please try again.</p>`;
    } finally {
        if (buttonElement) buttonElement.disabled = false;
    }
}

function getTripSummary(busId) {
    const bus = busData.find(b => b.id === busId);
    if (!bus) return;
    
    const summaryResultDiv = document.getElementById('gemini-summary-result');
    const summaryButton = document.getElementById('gemini-summary-btn');

    const prompt = `You are a friendly transit assistant. Provide a short, one-paragraph summary for a bus passenger about their upcoming trip. Be concise and conversational. Here is the trip info:
    - Bus Number: ${bus.number}
    - Route: ${bus.route}
    - Status: ${getStatusInfo(bus.status, bus.delay).text}
    - ETA: ${getPredictiveETA(bus.eta)} minutes
    - Crowd Level: ${getCrowdInfo(bus.crowd).text}
    - Key Stops: ${bus.stops.join(', ')}
    Generate a summary. Mention if they might get a seat or point out an interesting stop.`;

    generateGeminiContent(prompt, summaryResultDiv, summaryButton);
}
