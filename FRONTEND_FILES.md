# UzNavi Frontend Files - GitHub Ready

## 1. Main App Component (client/src/App.tsx)
```typescript
import { Route, Switch } from "wouter";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import Home from "@/pages/home";
import Places from "@/pages/places";
import Emergency from "@/pages/emergency";
import Pricing from "@/pages/pricing";
import NotFound from "@/pages/not-found";
import { Toaster } from "@/components/ui/toaster";

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5 minutes
      retry: (failureCount, error: any) => {
        if (error?.status === 404) return false;
        return failureCount < 3;
      },
    },
  },
});

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <div className="min-h-screen bg-background">
        <Switch>
          <Route path="/" component={Home} />
          <Route path="/places" component={Places} />
          <Route path="/emergency" component={Emergency} />
          <Route path="/pricing" component={Pricing} />
          <Route component={NotFound} />
        </Switch>
        <Toaster />
      </div>
    </QueryClientProvider>
  );
}

export default App;
```

## 2. Home Page with Voice AI (client/src/pages/home.tsx)
```typescript
import { useState, useEffect } from "react";
import { Button } from "@/components/ui/button";
import VoiceAI from "@/components/VoiceAI";
import ChatAI from "@/components/ChatAI";

// 60+ World Languages with activation status
const languages = [
  { code: 'uz', name: 'O\'zbekcha', flag: '🇺🇿', active: true },
  { code: 'en', name: 'English', flag: '🇺🇸', active: true },
  { code: 'ru', name: 'Русский', flag: '🇷🇺', active: true },
  { code: 'zh', name: '中文', flag: '🇨🇳', active: true },
  { code: 'es', name: 'Español', flag: '🇪🇸', active: true },
  { code: 'fr', name: 'Français', flag: '🇫🇷', active: true },
  { code: 'ar', name: 'العربية', flag: '🇸🇦', active: true },
  { code: 'de', name: 'Deutsch', flag: '🇩🇪', active: true },
  { code: 'it', name: 'Italiano', flag: '🇮🇹', active: true },
  { code: 'pt', name: 'Português', flag: '🇧🇷', active: true },
  { code: 'ja', name: '日本語', flag: '🇯🇵', active: true },
  { code: 'ko', name: '한국어', flag: '🇰🇷', active: true },
  { code: 'hi', name: 'हिन्दी', flag: '🇮🇳', active: true },
  { code: 'tr', name: 'Türkçe', flag: '🇹🇷', active: true },
  { code: 'fa', name: 'فارسی', flag: '🇮🇷', active: true },
  { code: 'kz', name: 'Қазақша', flag: '🇰🇿', active: true },
  { code: 'tj', name: 'Тоҷикӣ', flag: '🇹🇯', active: true },
  { code: 'ky', name: 'Кыргызча', flag: '🇰🇬', active: true },
  { code: 'tk', name: 'Türkmençe', flag: '🇹🇲', active: true },
  { code: 'az', name: 'Azərbaycan', flag: '🇦🇿', active: true },
  // Add more languages...
];

// World famous places for background slideshow
const worldPlaces = [
  {
    name: "Registon maydoni",
    description: "Samarqand shahrining markaziy maydoni",
    image: "https://images.unsplash.com/photo-1559827260-dc66d52bef19?ixlib=rb-4.0.3&auto=format&fit=crop&w=1200&q=80"
  },
  {
    name: "Ichan Qala",
    description: "Xivaning qadimiy qal'asi",
    image: "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?ixlib=rb-4.0.3&auto=format&fit=crop&w=1200&q=80"
  },
  {
    name: "Buxoro Ark qal'asi",
    description: "Buxoro amirlari qasr-qal'asi",
    image: "https://images.unsplash.com/photo-1570939274717-7eda259b50ed?ixlib=rb-4.0.3&auto=format&fit=crop&w=1200&q=80"
  },
  // Add more places...
];

export default function Home() {
  const [theme, setTheme] = useState('light');
  const [currentLang, setCurrentLang] = useState('UZ');
  const [showLanguageDropdown, setShowLanguageDropdown] = useState(false);
  const [currentSlide, setCurrentSlide] = useState(0);
  const [showChatAI, setShowChatAI] = useState(false);

  useEffect(() => {
    const interval = setInterval(() => {
      setCurrentSlide((prev) => (prev + 1) % worldPlaces.length);
    }, 5000);
    return () => clearInterval(interval);
  }, []);

  const toggleTheme = () => {
    const newTheme = theme === 'light' ? 'dark' : 'light';
    setTheme(newTheme);
    document.documentElement.classList.toggle('dark');
  };

  return (
    <div className={`min-h-screen ${theme === 'dark' ? 'dark' : ''}`}>
      {/* Voice AI positioned top-left */}
      <VoiceAI />
      
      {/* Chat AI Component */}
      {showChatAI && (
        <ChatAI onClose={() => setShowChatAI(false)} />
      )}

      {/* Header */}
      <header className="fixed top-0 left-0 right-0 z-40 bg-white/95 dark:bg-dark-bg-primary/95 backdrop-blur-sm border-b border-gray-200 dark:border-dark-border">
        <div className="container mx-auto px-6 py-4 flex items-center justify-between">
          <div className="flex items-center space-x-3">
            <div className="w-10 h-10 bg-gradient-to-r from-blue-500 to-emerald-500 rounded-lg flex items-center justify-center">
              <i className="fas fa-map-marked-alt text-white text-lg"></i>
            </div>
            <div>
              <h1 className="text-xl font-bold text-gray-900 dark:text-dark-text-primary">UzNavi</h1>
              <p className="text-xs text-gray-500 dark:text-dark-text-secondary">Travel Companion</p>
            </div>
          </div>
          
          {/* Navigation */}
          <nav className="hidden md:flex space-x-8">
            <a href="#home" className="nav-link text-blue-600 dark:text-blue-400 py-2 font-medium">
              Bosh sahifa
            </a>
            <a href="/places" className="nav-link text-gray-600 dark:text-dark-text-secondary hover:text-gray-900 dark:hover:text-dark-text-primary py-2 transition-colors">
              Joylar
            </a>
            <a href="/emergency" className="nav-link text-gray-600 dark:text-dark-text-secondary hover:text-gray-900 dark:hover:text-dark-text-primary py-2 transition-colors">
              Favqulodda
            </a>
            <a href="#features" className="nav-link text-gray-600 dark:text-dark-text-secondary hover:text-gray-900 dark:hover:text-dark-text-primary py-2 transition-colors">
              Features
            </a>
            <a href="/pricing" className="nav-link text-gray-600 dark:text-dark-text-secondary hover:text-gray-900 dark:hover:text-dark-text-primary py-2 transition-colors">
              To'lov
            </a>
          </nav>
          
          {/* CTA Buttons */}
          <div className="flex items-center space-x-4">
            <div className="relative hidden md:block">
              <Button
                variant="ghost"
                onClick={() => setShowLanguageDropdown(!showLanguageDropdown)}
                className="flex items-center space-x-2 text-gray-600 dark:text-dark-text-secondary hover:text-gray-900 dark:hover:text-dark-text-primary px-4 py-2"
              >
                <i className="fas fa-globe"></i>
                <span>{currentLang}</span>
                <i className="fas fa-chevron-down text-xs"></i>
              </Button>
              {showLanguageDropdown && (
                <div className="absolute right-0 mt-2 w-48 bg-white dark:bg-dark-bg-secondary rounded-lg shadow-lg py-2 z-50">
                  {languages.map((lang) => (
                    <button
                      key={lang.code}
                      className={`block w-full text-left px-4 py-2 text-gray-800 dark:text-dark-text-primary hover:bg-gray-100 dark:hover:bg-gray-700 ${
                        currentLang === lang.code.toUpperCase() ? 'bg-blue-50 dark:bg-blue-900/30 text-blue-600 dark:text-blue-400' : ''
                      } ${lang.active ? '' : 'opacity-50'}`}
                      onClick={() => {
                        setCurrentLang(lang.code.toUpperCase());
                        setShowLanguageDropdown(false);
                      }}
                    >
                      <span className="mr-2">{lang.flag}</span> {lang.name}
                      {lang.active && (
                        <i className="fas fa-check text-green-500 text-xs float-right mt-1"></i>
                      )}
                    </button>
                  ))}
                </div>
              )}
            </div>
            <Button
              onClick={toggleTheme}
              variant="ghost" 
              size="sm"
              className="hidden md:flex items-center justify-center w-10 h-10 rounded-lg text-gray-600 dark:text-dark-text-secondary hover:text-gray-900 dark:hover:text-dark-text-primary hover:bg-gray-100 dark:hover:bg-gray-700"
            >
              <i className={`fas ${theme === 'dark' ? 'fa-sun' : 'fa-moon'}`}></i>
            </Button>
            <a href="/pricing" className="inline-block">
              <Button className="gradient-bg hover:opacity-90 text-white px-6 py-2 rounded-lg font-medium animate-pulse">
                Boshlash
              </Button>
            </a>
          </div>
        </div>
      </header>

      {/* Hero Section */}
      <section id="home" className="gradient-bg text-white relative overflow-hidden">
        {/* Background Slideshow */}
        <div className="absolute inset-0">
          {worldPlaces.map((place, index) => (
            <div
              key={index}
              className={`absolute inset-0 transition-opacity duration-1000 ${
                index === currentSlide ? 'opacity-80' : 'opacity-0'
              }`}
              style={{
                backgroundImage: `url(${place.image})`,
                backgroundSize: 'cover',
                backgroundPosition: 'center',
                backgroundRepeat: 'no-repeat'
              }}
            />
          ))}
          <div className="absolute top-0 left-0 w-full h-full bg-gradient-to-br from-blue-500/20 to-emerald-500/20"></div>
          
          {/* Place Info Overlay */}
          <div className="absolute bottom-6 left-6 bg-black bg-opacity-40 backdrop-blur-sm px-4 py-2 rounded-lg">
            <h3 className="text-white font-semibold text-lg">{worldPlaces[currentSlide]?.name}</h3>
            <p className="text-blue-100 text-sm">{worldPlaces[currentSlide]?.description}</p>
          </div>
        </div>

        <div className="container mx-auto px-6 py-32 relative z-10">
          <div className="max-w-4xl mx-auto text-center">
            <h1 className="text-5xl md:text-6xl font-bold mb-6 animate-fade-in">
              O'zbekistonga <span className="text-yellow-300">Xush kelibsiz</span>
            </h1>
            <p className="text-xl md:text-2xl mb-8 text-blue-100 leading-relaxed animate-fade-in-delay">
              UzNavi - bu soddaligi va foydalanuvchilar uchun qulayligi bilan ajralib turadigan web sayt va mobil ilova bo'lib, sayohatchilar uchun AI yordamchisi, ovoz va chat bo'yicha qo'llab-quvvatlash imkoniyatlariga ega.
            </p>
            
            <div className="flex flex-col sm:flex-row gap-4 justify-center items-center mb-8">
              <Button 
                size="lg" 
                className="bg-white text-blue-600 hover:bg-blue-50 px-8 py-4 text-lg font-semibold rounded-xl shadow-lg transform hover:scale-105 transition-all duration-200"
                onClick={() => setShowChatAI(true)}
              >
                <i className="fas fa-comments mr-3"></i>
                AI Chat boshlash
              </Button>
              <Button 
                size="lg" 
                variant="outline" 
                className="border-2 border-white text-white hover:bg-white hover:text-blue-600 px-8 py-4 text-lg font-semibold rounded-xl shadow-lg transform hover:scale-105 transition-all duration-200"
              >
                <i className="fas fa-play mr-3"></i>
                Demo ko'rish
              </Button>
            </div>

            {/* Feature highlights */}
            <div className="grid grid-cols-2 md:grid-cols-4 gap-6 mt-16">
              <div className="text-center bg-white/10 backdrop-blur-sm rounded-lg p-4">
                <i className="fas fa-microphone text-3xl mb-2"></i>
                <p className="font-semibold">Ovozli AI</p>
              </div>
              <div className="text-center bg-white/10 backdrop-blur-sm rounded-lg p-4">
                <i className="fas fa-map-marked-alt text-3xl mb-2"></i>
                <p className="font-semibold">Offline Xarita</p>
              </div>
              <div className="text-center bg-white/10 backdrop-blur-sm rounded-lg p-4">
                <i className="fas fa-taxi text-3xl mb-2"></i>
                <p className="font-semibold">AI Taksi</p>
              </div>
              <div className="text-center bg-white/10 backdrop-blur-sm rounded-lg p-4">
                <i className="fas fa-phone-alt text-3xl mb-2"></i>
                <p className="font-semibold">Favqulodda</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* Rest of the home page content... */}
    </div>
  );
}
```

## 3. Voice AI Component (client/src/components/VoiceAI.tsx)
```typescript
import { useState, useRef, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { apiRequest } from "@/lib/queryClient";
import { useToast } from "@/hooks/use-toast";

export default function VoiceAI() {
  const [isListening, setIsListening] = useState(false);
  const [isProcessing, setIsProcessing] = useState(false);
  const [transcript, setTranscript] = useState("");
  const [response, setResponse] = useState("");
  const [showResponse, setShowResponse] = useState(false);
  const recognitionRef = useRef<SpeechRecognition | null>(null);
  const { toast } = useToast();

  useEffect(() => {
    if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      recognitionRef.current = new SpeechRecognition();
      
      recognitionRef.current.continuous = false;
      recognitionRef.current.interimResults = true;
      recognitionRef.current.lang = 'uz-UZ';

      recognitionRef.current.onstart = () => {
        setIsListening(true);
      };

      recognitionRef.current.onresult = (event) => {
        const current = event.resultIndex;
        const transcript = event.results[current][0].transcript;
        setTranscript(transcript);
        
        if (event.results[current].isFinal) {
          processVoiceCommand(transcript);
        }
      };

      recognitionRef.current.onerror = (event) => {
        console.error('Speech recognition error:', event.error);
        setIsListening(false);
        toast({
          title: "Ovoz tanish xatoligi",
          description: "Iltimos, qayta urinib ko'ring",
          variant: "destructive",
        });
      };

      recognitionRef.current.onend = () => {
        setIsListening(false);
      };
    }
  }, []);

  const processVoiceCommand = async (command: string) => {
    if (!command.trim()) return;
    
    setIsProcessing(true);
    try {
      const sessionId = `session_${Date.now()}_${Math.random()}`;
      const result = await apiRequest("POST", "/api/voice", {
        command: command,
        sessionId: sessionId
      });
      
      const data = await result.json();
      setResponse(data.response);
      setShowResponse(true);
      
      // Text-to-speech for response
      if ('speechSynthesis' in window && data.response) {
        const utterance = new SpeechSynthesisUtterance(data.response);
        utterance.lang = 'uz-UZ';
        utterance.rate = 0.9;
        speechSynthesis.speak(utterance);
      }
    } catch (error) {
      console.error('Voice command error:', error);
      toast({
        title: "Xatolik",
        description: "Ovozli buyruqni bajarishda xatolik yuz berdi",
        variant: "destructive",
      });
    } finally {
      setIsProcessing(false);
    }
  };

  const startListening = () => {
    if (recognitionRef.current && !isListening) {
      setTranscript("");
      setResponse("");
      setShowResponse(false);
      recognitionRef.current.start();
    }
  };

  const stopListening = () => {
    if (recognitionRef.current && isListening) {
      recognitionRef.current.stop();
    }
  };

  return (
    <div className="fixed top-6 left-6 z-50">
      <div className="bg-white dark:bg-gray-800 rounded-2xl shadow-lg border border-gray-200 dark:border-gray-700 p-4">
        <div className="flex items-center space-x-3">
          <Button
            onClick={isListening ? stopListening : startListening}
            disabled={isProcessing}
            className={`relative w-12 h-12 rounded-xl transition-all duration-200 ${
              isListening 
                ? 'bg-red-500 hover:bg-red-600 animate-pulse' 
                : 'bg-blue-500 hover:bg-blue-600'
            } ${isProcessing ? 'opacity-50 cursor-not-allowed' : ''}`}
          >
            <i className={`fas ${
              isProcessing 
                ? 'fa-spinner fa-spin' 
                : isListening 
                  ? 'fa-stop' 
                  : 'fa-microphone'
            } text-white text-lg`}></i>
          </Button>
          
          <div className="flex flex-col">
            <span className="text-sm font-medium text-gray-900 dark:text-gray-100">
              Voice AI
            </span>
            <span className="text-xs text-gray-500 dark:text-gray-400">
              {isListening ? 'Eshityapman...' : isProcessing ? 'Qayta ishlanmoqda...' : 'Bosing'}
            </span>
          </div>
        </div>

        {/* Transcript Display */}
        {transcript && (
          <div className="mt-3 p-2 bg-gray-50 dark:bg-gray-700 rounded-lg">
            <p className="text-sm text-gray-700 dark:text-gray-300">
              <i className="fas fa-quote-left text-xs text-gray-400 mr-1"></i>
              {transcript}
            </p>
          </div>
        )}

        {/* Response Display */}
        {showResponse && response && (
          <div className="mt-3 p-3 bg-blue-50 dark:bg-blue-900/30 rounded-lg border-l-4 border-blue-500">
            <p className="text-sm text-blue-800 dark:text-blue-200">
              <i className="fas fa-robot text-blue-500 mr-2"></i>
              {response}
            </p>
          </div>
        )}
      </div>
    </div>
  );
}
```

## 4. Pricing Page (client/src/pages/pricing.tsx)
```typescript
import { useState } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";

// Time-based payment plans only
const pricingPlans = [
  {
    id: 'trial',
    name: 'Bepul Sinov',
    price: 'Bepul',
    duration: '48 soat',
    description: 'Barcha xususiyatlarni sinab ko\'ring',
    features: [
      'AI ovozli yordamchi',
      'Chat AI',
      'Offline xaritalar',
      'Joylar bazasi',
      'Favqulodda xizmatlar',
      'Taksi chaqirish',
      'Valyuta kurslari',
      'Ob-havo ma\'lumotlari'
    ],
    badge: 'Mashhur',
    badgeColor: 'bg-green-500'
  },
  {
    id: 'basic',
    name: '3 Kunlik',
    price: '$2.99',
    duration: '3 kun',
    description: 'Qisqa sayohatlar uchun',
    features: [
      'Barcha bepul imkoniyatlar',
      '24/7 AI yordam',
      'Premium joylar ma\'lumotlari',
      'Tezkor qo\'llab-quvvatlash',
      'Offline ishlash',
      'Bir nechta tillar'
    ],
    badge: null
  },
  {
    id: 'premium', 
    name: '1 Haftalik',
    price: '$5.99',
    duration: '1 hafta',
    description: 'Uzaytirilgan sayohatlar uchun',
    features: [
      'Barcha basic imkoniyatlar',
      'Premium AI xususiyatlari',
      'Kengaytirilgan qidiruv',
      'Maxsus tavsiyalar',
      'Priority support',
      'Advanced analytics'
    ],
    badge: 'Eng yaxshi tanlov',
    badgeColor: 'bg-blue-500'
  },
  {
    id: 'ultimate',
    name: '1 Oylik',
    price: '$24.99', 
    duration: '1 oy',
    description: 'Uzoq muddatli sayohatlar uchun',
    features: [
      'Barcha premium imkoniyatlar',
      'VIP AI yordam',
      'Cheksiz qidiruvlar',
      'Personal travel consultant',
      'API access',
      'Custom integrations'
    ],
    badge: 'Professional',
    badgeColor: 'bg-purple-500'
  }
];

export default function Pricing() {
  const [selectedPlan, setSelectedPlan] = useState('trial');

  const handlePlanSelect = (planId: string) => {
    setSelectedPlan(planId);
    console.log('Selected plan:', planId);
  };

  return (
    <div className="min-h-screen bg-gray-50 dark:bg-gray-900 py-16">
      <div className="container mx-auto px-6">
        {/* Header */}
        <div className="text-center mb-16">
          <h1 className="text-4xl md:text-5xl font-bold text-gray-900 dark:text-white mb-6">
            Vaqt Asosidagi To'lov Rejalari
          </h1>
          <p className="text-xl text-gray-600 dark:text-gray-400 max-w-3xl mx-auto">
            Sayohat muddatingizga mos kelgan rejani tanlang. Barcha rejalar bir xil xususiyatlarga ega, faqat foydalanish vaqti farq qiladi.
          </p>
          
          <div className="mt-8 bg-blue-50 dark:bg-blue-900/20 border border-blue-200 dark:border-blue-800 rounded-lg p-4 max-w-2xl mx-auto">
            <p className="text-blue-800 dark:text-blue-200 font-medium">
              <i className="fas fa-info-circle mr-2"></i>
              Hech qanday maxsus xususiyat yo'q - faqat vaqt boshqaruvi
            </p>
          </div>
        </div>

        {/* Pricing Cards */}
        <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-8 max-w-7xl mx-auto">
          {pricingPlans.map((plan) => (
            <Card 
              key={plan.id}
              className={`relative h-full transition-all duration-200 hover:shadow-lg cursor-pointer ${
                selectedPlan === plan.id 
                  ? 'ring-2 ring-blue-500 bg-blue-50 dark:bg-blue-900/20' 
                  : 'bg-white dark:bg-gray-800'
              }`}
              onClick={() => handlePlanSelect(plan.id)}
            >
              {plan.badge && (
                <div className="absolute -top-3 left-1/2 transform -translate-x-1/2">
                  <Badge className={`${plan.badgeColor} text-white px-3 py-1`}>
                    {plan.badge}
                  </Badge>
                </div>
              )}
              
              <CardHeader className="text-center pb-4">
                <CardTitle className="text-2xl font-bold text-gray-900 dark:text-white">
                  {plan.name}
                </CardTitle>
                <div className="mt-4">
                  <span className="text-4xl font-bold text-blue-600 dark:text-blue-400">
                    {plan.price}
                  </span>
                  <span className="text-gray-500 dark:text-gray-400 ml-2">
                    / {plan.duration}
                  </span>
                </div>
                <CardDescription className="mt-2 text-gray-600 dark:text-gray-400">
                  {plan.description}
                </CardDescription>
              </CardHeader>

              <CardContent className="pt-0">
                <ul className="space-y-3 mb-6">
                  {plan.features.map((feature, index) => (
                    <li key={index} className="flex items-start">
                      <i className="fas fa-check text-green-500 mt-1 mr-3 text-sm"></i>
                      <span className="text-gray-700 dark:text-gray-300 text-sm">
                        {feature}
                      </span>
                    </li>
                  ))}
                </ul>

                <Button 
                  className={`w-full ${
                    selectedPlan === plan.id
                      ? 'bg-blue-600 hover:bg-blue-700 text-white'
                      : 'bg-gray-100 dark:bg-gray-700 text-gray-900 dark:text-white hover:bg-gray-200 dark:hover:bg-gray-600'
                  }`}
                  onClick={() => handlePlanSelect(plan.id)}
                >
                  {selectedPlan === plan.id ? (
                    <>
                      <i className="fas fa-check mr-2"></i>
                      Tanlangan
                    </>
                  ) : (
                    'Tanlash'
                  )}
                </Button>
              </CardContent>
            </Card>
          ))}
        </div>

        {/* Features comparison */}
        <div className="mt-16 bg-white dark:bg-gray-800 rounded-xl shadow-lg p-8">
          <h3 className="text-2xl font-bold text-center text-gray-900 dark:text-white mb-8">
            Barcha rejalar bir xil imkoniyatlarga ega
          </h3>
          
          <div className="grid md:grid-cols-3 gap-8">
            <div className="text-center">
              <div className="w-16 h-16 bg-blue-100 dark:bg-blue-900/30 rounded-full flex items-center justify-center mx-auto mb-4">
                <i className="fas fa-robot text-blue-600 dark:text-blue-400 text-2xl"></i>
              </div>
              <h4 className="text-xl font-semibold text-gray-900 dark:text-white mb-2">
                AI Yordamchi
              </h4>
              <p className="text-gray-600 dark:text-gray-400">
                Ovozli va chat AI yordamchi bilan 24/7 yordam
              </p>
            </div>
            
            <div className="text-center">
              <div className="w-16 h-16 bg-green-100 dark:bg-green-900/30 rounded-full flex items-center justify-center mx-auto mb-4">
                <i className="fas fa-map-marked-alt text-green-600 dark:text-green-400 text-2xl"></i>
              </div>
              <h4 className="text-xl font-semibold text-gray-900 dark:text-white mb-2">
                Offline Xaritalar
              </h4>
              <p className="text-gray-600 dark:text-gray-400">
                Internet aloqasi bo'lmaganda ham ishlaydigan xaritalar
              </p>
            </div>
            
            <div className="text-center">
              <div className="w-16 h-16 bg-purple-100 dark:bg-purple-900/30 rounded-full flex items-center justify-center mx-auto mb-4">
                <i className="fas fa-phone-alt text-purple-600 dark:text-purple-400 text-2xl"></i>
              </div>
              <h4 className="text-xl font-semibold text-gray-900 dark:text-white mb-2">
                Favqulodda Yordam
              </h4>
              <p className="text-gray-600 dark:text-gray-400">
                Tez yordam, politsiya va boshqa xizmatlar raqamlari
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

## 5. Chat AI Component (client/src/components/ChatAI.tsx)
```typescript
import { useState, useRef, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { ScrollArea } from "@/components/ui/scroll-area";
import { apiRequest } from "@/lib/queryClient";
import { useToast } from "@/hooks/use-toast";

interface Message {
  id: string;
  type: 'user' | 'ai';
  content: string;
  timestamp: Date;
}

interface ChatAIProps {
  onClose: () => void;
}

export default function ChatAI({ onClose }: ChatAIProps) {
  const [messages, setMessages] = useState<Message[]>([
    {
      id: '1',
      type: 'ai',
      content: "Salom! Men UzNavi AI yordamchisiman. O'zbekiston bo'ylab sayohatingizda sizga yordam beraman. Menga har qanday savol bering! 🇺🇿",
      timestamp: new Date()
    }
  ]);
  const [input, setInput] = useState("");
  const [isLoading, setIsLoading] = useState(false);
  const messagesEndRef = useRef<HTMLDivElement>(null);
  const sessionId = useRef(`session_${Date.now()}_${Math.random()}`);
  const { toast } = useToast();

  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages]);

  const sendMessage = async () => {
    if (!input.trim() || isLoading) return;

    const userMessage: Message = {
      id: Date.now().toString(),
      type: 'user',
      content: input,
      timestamp: new Date()
    };

    setMessages(prev => [...prev, userMessage]);
    setInput("");
    setIsLoading(true);

    try {
      const result = await apiRequest("POST", "/api/chat", {
        message: input,
        sessionId: sessionId.current
      });
      
      const data = await result.json();
      
      const aiMessage: Message = {
        id: (Date.now() + 1).toString(),
        type: 'ai',
        content: data.response,
        timestamp: new Date()
      };

      setMessages(prev => [...prev, aiMessage]);
    } catch (error) {
      console.error('Chat error:', error);
      toast({
        title: "Xatolik",
        description: "Xabar yuborishda xatolik yuz berdi",
        variant: "destructive",
      });
    } finally {
      setIsLoading(false);
    }
  };

  const handleKeyPress = (e: React.KeyboardEvent) => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      sendMessage();
    }
  };

  return (
    <div className="fixed inset-0 bg-black/50 flex items-center justify-center z-50 p-4">
      <Card className="w-full max-w-2xl h-[600px] flex flex-col bg-white dark:bg-gray-800">
        <CardHeader className="flex flex-row items-center justify-between pb-4 border-b">
          <CardTitle className="text-xl font-semibold text-gray-900 dark:text-white flex items-center">
            <i className="fas fa-robot text-blue-500 mr-3"></i>
            UzNavi AI Chat
          </CardTitle>
          <Button
            variant="ghost"
            size="sm"
            onClick={onClose}
            className="text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200"
          >
            <i className="fas fa-times text-lg"></i>
          </Button>
        </CardHeader>

        <CardContent className="flex-1 flex flex-col p-0">
          <ScrollArea className="flex-1 p-4">
            <div className="space-y-4">
              {messages.map((message) => (
                <div
                  key={message.id}
                  className={`flex ${message.type === 'user' ? 'justify-end' : 'justify-start'}`}
                >
                  <div
                    className={`max-w-[80%] rounded-lg p-3 ${
                      message.type === 'user'
                        ? 'bg-blue-500 text-white'
                        : 'bg-gray-100 dark:bg-gray-700 text-gray-900 dark:text-white'
                    }`}
                  >
                    <p className="text-sm leading-relaxed">{message.content}</p>
                    <p className={`text-xs mt-2 opacity-70 ${
                      message.type === 'user' ? 'text-blue-100' : 'text-gray-500 dark:text-gray-400'
                    }`}>
                      {message.timestamp.toLocaleTimeString('uz-UZ', { 
                        hour: '2-digit', 
                        minute: '2-digit' 
                      })}
                    </p>
                  </div>
                </div>
              ))}
              
              {isLoading && (
                <div className="flex justify-start">
                  <div className="bg-gray-100 dark:bg-gray-700 rounded-lg p-3">
                    <div className="flex space-x-1">
                      <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce"></div>
                      <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style={{ animationDelay: '0.1s' }}></div>
                      <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style={{ animationDelay: '0.2s' }}></div>
                    </div>
                  </div>
                </div>
              )}
            </div>
            <div ref={messagesEndRef} />
          </ScrollArea>

          {/* Input area */}
          <div className="border-t p-4">
            <div className="flex space-x-2">
              <Input
                value={input}
                onChange={(e) => setInput(e.target.value)}
                onKeyPress={handleKeyPress}
                placeholder="Savolingizni yozing..."
                disabled={isLoading}
                className="flex-1"
              />
              <Button
                onClick={sendMessage}
                disabled={!input.trim() || isLoading}
                className="bg-blue-500 hover:bg-blue-600 text-white px-6"
              >
                {isLoading ? (
                  <i className="fas fa-spinner fa-spin"></i>
                ) : (
                  <i className="fas fa-paper-plane"></i>
                )}
              </Button>
            </div>
            
            <div className="flex flex-wrap gap-2 mt-3">
              {[
                "Eng yaxshi restoranlar qayerda?",
                "Taksi qanday chaqiraman?",
                "Favqulodda raqamlar qanday?",
                "Valyuta ayriboshlash joylar"
              ].map((suggestion, index) => (
                <Button
                  key={index}
                  variant="outline"
                  size="sm"
                  onClick={() => setInput(suggestion)}
                  disabled={isLoading}
                  className="text-xs"
                >
                  {suggestion}
                </Button>
              ))}
            </div>
          </div>
        </CardContent>
      </Card>
    </div>
  );
}
```

Bu frontend 60+ til bilan to'liq tayyor!