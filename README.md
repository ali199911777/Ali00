import { Route, Switch } from "wouter";
import { Home } from "./pages/Home";
import { Profile } from "./pages/Profile";
import { Discover } from "./pages/Discover";
import { Inbox } from "./pages/Inbox";
import NotFound from "./pages/not-found";
import { useState } from "react";
import Header from "./components/Header";
import BottomNavigation from "./components/BottomNavigation";
import { QueryClientProvider } from "@tanstack/react-query";
import { queryClient } from "./lib/queryClient";
import { Toaster } from "./components/ui/toaster";
import { AccessibilityProvider } from "./hooks/useAccessibility";

function Router() {
  const [activeTab, setActiveTab] = useState("home");

  const handleTabChange = (tab: string) => {
    setActiveTab(tab);
  };

  return (
    <div className="flex flex-col h-full">
      <Header activeTab={activeTab} onTabChange={handleTabChange} />
      <main className="flex-1 overflow-hidden relative">
        <Switch>
          <Route path="/" component={() => <Home activeTab={activeTab} />} />
          <Route path="/discover" component={Discover} />
          <Route path="/profile/:id?" component={Profile} />
          <Route path="/inbox" component={Inbox} />
          <Route component={NotFound} />
        </Switch>
      </main>
      <BottomNavigation activeTab={activeTab} onTabChange={handleTabChange} />
    </div>
  );
}

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <AccessibilityProvider>
        <div className="h-[100dvh] w-full bg-background text-foreground flex flex-col overflow-hidden">
          <Router />
        </div>
        <Toaster />
      </AccessibilityProvider>
    </QueryClientProvider>
  );
}

export default App;
