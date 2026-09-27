--- src/App.tsx (原始)
import React, { useState } from 'react';
import { AppProvider } from './store/AppContext';
import DashboardPage from './pages/DashboardPage';
import POSPage from './pages/POSPage';
import ProductsPage from './pages/ProductsPage';
import IngredientsPage from './pages/IngredientsPage';
import PurchasesPage from './pages/PurchasesPage';
import ReportsPage from './pages/ReportsPage';
import DebtsPage from './pages/DebtsPage';
import SettingsPage from './pages/SettingsPage';
import {
  LayoutDashboard, ShoppingCart, Package, ChefHat,
  ShoppingBag, BarChart3, CreditCard, Settings, Menu, X, Coffee
} from 'lucide-react';

type Page = 'dashboard' | 'pos' | 'products' | 'ingredients' | 'purchases' | 'reports' | 'debts' | 'settings';

const navItems: { id: Page; label: string; icon: React.ReactNode }[] = [
  { id: 'dashboard', label: 'Dashboard', icon: <LayoutDashboard className="w-5 h-5" /> },
  { id: 'pos', label: 'Cashier', icon: <ShoppingCart className="w-5 h-5" /> },
  { id: 'products', label: 'Products', icon: <Coffee className="w-5 h-5" /> },
  { id: 'ingredients', label: 'Ingredients', icon: <ChefHat className="w-5 h-5" /> },
  { id: 'purchases', label: 'Purchases', icon: <ShoppingBag className="w-5 h-5" /> },
  { id: 'reports', label: 'Reports', icon: <BarChart3 className="w-5 h-5" /> },
  { id: 'debts', label: 'Debts', icon: <CreditCard className="w-5 h-5" /> },
  { id: 'settings', label: 'Settings', icon: <Settings className="w-5 h-5" /> },
];

function AppContent() {
  const [currentPage, setCurrentPage] = useState<Page>('dashboard');
  const [sidebarOpen, setSidebarOpen] = useState(false);

  const renderPage = () => {
    switch (currentPage) {
      case 'dashboard': return <DashboardPage />;
      case 'pos': return <POSPage />;
      case 'products': return <ProductsPage />;
      case 'ingredients': return <IngredientsPage />;
      case 'purchases': return <PurchasesPage />;
      case 'reports': return <ReportsPage />;
      case 'debts': return <DebtsPage />;
      case 'settings': return <SettingsPage />;
      default: return <DashboardPage />;
    }
  };

  return (
    <div className="flex h-screen bg-gray-50">
      {/* Mobile Overlay */}
      {sidebarOpen && (
        <div
          className="fixed inset-0 bg-black/50 z-40 lg:hidden"
          onClick={() => setSidebarOpen(false)}
        />
      )}

      {/* Sidebar */}
      <aside className={`fixed lg:static inset-y-0 left-0 z-50 w-64 bg-white border-r border-gray-200 transform transition-transform duration-200 ${
        sidebarOpen ? 'translate-x-0' : '-translate-x-full lg:translate-x-0'
      }`}>
        <div className="flex items-center gap-3 p-4 border-b border-gray-100">
          <div className="w-10 h-10 bg-amber-600 rounded-xl flex items-center justify-center">
            <Coffee className="w-6 h-6 text-white" />
          </div>
          <div>
            <h1 className="font-bold text-gray-800 text-sm">Coffee Shop</h1>
            <p className="text-xs text-gray-500">POS System</p>
          </div>
          <button onClick={() => setSidebarOpen(false)} className="ml-auto lg:hidden text-gray-400">
            <X className="w-5 h-5" />
          </button>
        </div>

        <nav className="p-3 space-y-1">
          {navItems.map(item => (
            <button
              key={item.id}
              onClick={() => { setCurrentPage(item.id); setSidebarOpen(false); }}
              className={`w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium transition ${
                currentPage === item.id
                  ? 'bg-amber-50 text-amber-700'
                  : 'text-gray-600 hover:bg-gray-50 hover:text-gray-800'
              }`}
            >
              {item.icon}
              {item.label}
            </button>
          ))}
        </nav>

        <div className="absolute bottom-4 left-4 right-4">
          <div className="bg-gradient-to-br from-amber-50 to-orange-50 rounded-xl p-3">
            <p className="text-xs text-amber-700 font-medium">Coffee Shop POS</p>
            <p className="text-xs text-amber-600 mt-0.5">v1.0 • PWA Ready</p>
          </div>
        </div>
      </aside>

      {/* Main Content */}
      <main className="flex-1 flex flex-col overflow-hidden">
        {/* Top Bar */}
        <header className="bg-white border-b border-gray-200 px-4 py-3 flex items-center gap-4">
          <button
            onClick={() => setSidebarOpen(true)}
            className="lg:hidden text-gray-600"
          >
            <Menu className="w-6 h-6" />
          </button>
          <div>
            <h2 className="font-medium text-gray-800 capitalize">
              {currentPage === 'pos' ? 'Cashier / POS' : navItems.find(n => n.id === currentPage)?.label}
            </h2>
          </div>
          <div className="ml-auto flex items-center gap-2">
            <span className="text-xs text-gray-400 hidden sm:block">
              {new Date().toLocaleDateString('en-GB', { weekday: 'short', day: 'numeric', month: 'short' })}
            </span>
          </div>
        </header>

        {/* Page Content */}
        <div className="flex-1 overflow-y-auto p-4 lg:p-6">
          {renderPage()}
        </div>
      </main>
    </div>
  );
}

export default function App() {
  return (
    <AppProvider>
      <AppContent />
    </AppProvider>
  );
}


+++ src/App.tsx (修改后)
import React, { useState } from 'react';
import { AppProvider, useAppState } from './store/AppContext';
import DashboardPage from './pages/DashboardPage';
import POSPage from './pages/POSPage';
import ProductsPage from './pages/ProductsPage';
import IngredientsPage from './pages/IngredientsPage';
import PurchasesPage from './pages/PurchasesPage';
import ReportsPage from './pages/ReportsPage';
import DebtsPage from './pages/DebtsPage';
import SettingsPage from './pages/SettingsPage';
import {
  LayoutDashboard, ShoppingCart, Package, ChefHat,
  ShoppingBag, BarChart3, CreditCard, Settings, Menu, X, Coffee
} from 'lucide-react';

type Page = 'dashboard' | 'pos' | 'products' | 'ingredients' | 'purchases' | 'reports' | 'debts' | 'settings';

const navItems: { id: Page; label: string; icon: React.ReactNode }[] = [
  { id: 'dashboard', label: 'Dashboard', icon: <LayoutDashboard className="w-5 h-5" /> },
  { id: 'pos', label: 'Cashier', icon: <ShoppingCart className="w-5 h-5" /> },
  { id: 'products', label: 'Products', icon: <Coffee className="w-5 h-5" /> },
  { id: 'ingredients', label: 'Ingredients', icon: <ChefHat className="w-5 h-5" /> },
  { id: 'purchases', label: 'Purchases', icon: <ShoppingBag className="w-5 h-5" /> },
  { id: 'reports', label: 'Reports', icon: <BarChart3 className="w-5 h-5" /> },
  { id: 'debts', label: 'Debts', icon: <CreditCard className="w-5 h-5" /> },
  { id: 'settings', label: 'Settings', icon: <Settings className="w-5 h-5" /> },
];

function AppContent() {
  const { state } = useAppState();
  const [currentPage, setCurrentPage] = useState<Page>('dashboard');
  const [sidebarOpen, setSidebarOpen] = useState(false);

  const renderPage = () => {
    switch (currentPage) {
      case 'dashboard': return <DashboardPage />;
      case 'pos': return <POSPage />;
      case 'products': return <ProductsPage />;
      case 'ingredients': return <IngredientsPage />;
      case 'purchases': return <PurchasesPage />;
      case 'reports': return <ReportsPage />;
      case 'debts': return <DebtsPage />;
      case 'settings': return <SettingsPage />;
      default: return <DashboardPage />;
    }
  };

  return (
    <div className="flex h-screen bg-gray-50">
      {/* Mobile Overlay */}
      {sidebarOpen && (
        <div
          className="fixed inset-0 bg-black/50 z-40 lg:hidden"
          onClick={() => setSidebarOpen(false)}
        />
      )}

      {/* Sidebar */}
      <aside className={`fixed lg:static inset-y-0 left-0 z-50 w-64 bg-white border-r border-gray-200 transform transition-transform duration-200 ${
        sidebarOpen ? 'translate-x-0' : '-translate-x-full lg:translate-x-0'
      }`}>
        <div className="flex items-center gap-3 p-4 border-b border-gray-100">
          <div className="w-10 h-10 bg-amber-600 rounded-xl flex items-center justify-center">
            <Coffee className="w-6 h-6 text-white" />
          </div>
          <div>
            <h1 className="font-bold text-gray-800 text-sm">{state.settings.storeName || 'Coffee Shop'}</h1>
            <p className="text-xs text-gray-500">{state.settings.ownerName ? `Owner: ${state.settings.ownerName}` : 'POS System'}</p>
          </div>
          <button onClick={() => setSidebarOpen(false)} className="ml-auto lg:hidden text-gray-400">
            <X className="w-5 h-5" />
          </button>
        </div>

        <nav className="p-3 space-y-1">
          {navItems.map(item => (
            <button
              key={item.id}
              onClick={() => { setCurrentPage(item.id); setSidebarOpen(false); }}
              className={`w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium transition ${
                currentPage === item.id
                  ? 'bg-amber-50 text-amber-700'
                  : 'text-gray-600 hover:bg-gray-50 hover:text-gray-800'
              }`}
            >
              {item.icon}
              {item.label}
            </button>
          ))}
        </nav>

        <div className="absolute bottom-4 left-4 right-4">
          <div className="bg-gradient-to-br from-amber-50 to-orange-50 rounded-xl p-3">
            <p className="text-xs text-amber-700 font-medium">Coffee Shop POS</p>
            <p className="text-xs text-amber-600 mt-0.5">v1.0 • PWA Ready</p>
          </div>
        </div>
      </aside>

      {/* Main Content */}
      <main className="flex-1 flex flex-col overflow-hidden">
        {/* Top Bar */}
        <header className="bg-white border-b border-gray-200 px-4 py-3 flex items-center gap-4">
          <button
            onClick={() => setSidebarOpen(true)}
            className="lg:hidden text-gray-600"
          >
            <Menu className="w-6 h-6" />
          </button>
          <div>
            <h2 className="font-medium text-gray-800 capitalize">
              {currentPage === 'pos' ? 'Cashier / POS' : navItems.find(n => n.id === currentPage)?.label}
            </h2>
          </div>
          <div className="ml-auto flex items-center gap-2">
            <span className="text-xs text-gray-400 hidden sm:block">
              {new Date().toLocaleDateString('en-GB', { weekday: 'short', day: 'numeric', month: 'short' })}
            </span>
          </div>
        </header>

        {/* Page Content */}
        <div className="flex-1 overflow-y-auto p-4 lg:p-6">
          {renderPage()}
        </div>
      </main>
    </div>
  );
}

export default function App() {
  return (
    <AppProvider>
      <AppContent />
    </AppProvider>
  );
}
