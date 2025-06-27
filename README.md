# Advanced Expo React Native Coding Challenge

## Time Limit: 90 minutes

---

## Challenge: Real-Time Collaborative Trading Platform

Build a sophisticated trading platform that handles real-time market data, collaborative features, and complex financial calculations - all within Expo's constraints.

### Core Requirements

You must implement a complete trading interface with the following features:

#### 1. Real-Time Market Data (25 points)
```typescript
interface MarketDataStream {
  symbol: string;
  price: number;
  change: number;
  changePercent: number;
  volume: number;
  timestamp: number;
  bidAsk: { bid: number; ask: number };
  marketCap?: number;
  pe?: number;
}

interface TradingPlatform {
  // Handle 100+ symbols updating every 100ms
  subscribeToMarketData(symbols: string[]): void;
  
  // Real-time price charts with technical indicators
  renderPriceChart(symbol: string, timeframe: string): React.ReactElement;
  
  // Streaming watchlist with instant updates
  updateWatchlist(symbols: string[]): void;
}
```

#### 2. Advanced Portfolio Management (25 points)
```typescript
interface Portfolio {
  positions: Position[];
  totalValue: number;
  dayChange: number;
  dayChangePercent: number;
  availableCash: number;
  marginUsed: number;
}

interface Position {
  symbol: string;
  quantity: number;
  avgCost: number;
  currentPrice: number;
  unrealizedPL: number;
  realizedPL: number;
  dayChange: number;
  allocation: number; // percentage of portfolio
}

// Complex calculations required:
interface PortfolioCalculations {
  calculateRisk(): PortfolioRisk;
  calculateSharpeRatio(): number;
  calculateMaxDrawdown(): number;
  calculateBeta(): number;
  rebalancePortfolio(targetAllocations: AllocationTarget[]): RebalanceOrder[];
}
```

#### 3. Collaborative Trading Rooms (25 points)
```typescript
interface TradingRoom {
  id: string;
  name: string;
  members: TradingMember[];
  sharedWatchlist: string[];
  chatMessages: ChatMessage[];
  sharedAnalysis: Analysis[];
}

interface CollaborativeFeatures {
  // Real-time cursor tracking on charts
  showMemberCursors(roomId: string): void;
  
  // Shared annotations on price charts
  syncChartAnnotations(annotations: ChartAnnotation[]): void;
  
  // Live trading signals
  broadcastTradingSignal(signal: TradingSignal): void;
  
  // Collaborative order book
  shareOrderIntent(order: OrderIntent): void;
}
```

#### 4. Advanced Order Management (25 points)
```typescript
interface OrderManagement {
  // Complex order types
  createBracketOrder(entry: Order, stopLoss: Order, takeProfit: Order): Promise<OrderGroup>;
  createTrailingStop(symbol: string, trailAmount: number): Promise<Order>;
  createIcebergOrder(symbol: string, totalQuantity: number, displayQuantity: number): Promise<Order>;
  
  // Risk management
  validateOrderRisk(order: Order): RiskValidation;
  calculatePositionSize(riskAmount: number, stopLoss: number): number;
  
  // Order execution algorithms
  executeVWAPOrder(symbol: string, quantity: number, timeframe: number): Promise<ExecutionReport>;
  executeTWAPOrder(symbol: string, quantity: number, duration: number): Promise<ExecutionReport>;
}
```

---

## Technical Implementation Requirements

### 1. Real-Time Data Architecture
```typescript
// Implement efficient WebSocket management
class MarketDataManager {
  private connections: Map<string, WebSocket>;
  private subscriptions: Map<string, Set<string>>;
  private dataBuffer: Map<string, MarketDataStream[]>;
  
  // Handle connection failures and reconnection
  private async reconnectWithBackoff(url: string): Promise<void>;
  
  // Implement data compression and batching
  private compressMarketData(data: MarketDataStream[]): CompressedData;
  
  // Memory management for streaming data
  private cleanupOldData(): void;
  
  // Handle rate limiting and throttling
  private throttleUpdates(symbol: string, data: MarketDataStream): void;
}
```

### 2. Performance Optimization
```typescript
// Virtualization for large lists
interface VirtualizedWatchlist {
  // Render only visible items from 1000+ symbols
  renderItem: (item: MarketDataStream, index: number) => React.ReactElement;
  
  // Smooth scrolling with momentum
  onScroll: (event: ScrollEvent) => void;
  
  // Dynamic height calculation
  getItemHeight: (index: number) => number;
}

// Chart performance optimization
interface HighPerformanceChart {
  // Render 10,000+ candlesticks smoothly
  renderCandlesticks(data: OHLCV[]): void;
  
  // Real-time updates without full re-render
  updateLatestCandle(candle: OHLCV): void;
  
  // Gesture handling for zoom/pan
  handleChartGestures(gesture: PanGesture | PinchGesture): void;
  
  // Technical indicators overlay
  renderIndicators(indicators: TechnicalIndicator[]): void;
}
```

### 3. State Management Architecture
```typescript
// Complex state synchronization
interface TradingAppState {
  marketData: Map<string, MarketDataStream>;
  portfolio: Portfolio;
  orders: Order[];
  tradingRooms: TradingRoom[];
  userPreferences: UserPreferences;
  notifications: Notification[];
  chartStates: Map<string, ChartState>;
}

// Implement without external state libraries
class StateManager {
  private state: TradingAppState;
  private listeners: Map<string, Function[]>;
  private persistence: AsyncStorage;
  
  // Optimistic updates with rollback
  async updateWithOptimism<T>(
    key: keyof TradingAppState,
    update: (current: T) => T,
    serverUpdate: () => Promise<T>
  ): Promise<void>;
  
  // Conflict resolution for collaborative features
  resolveStateConflict(local: any, remote: any, timestamp: number): any;
  
  // Memory-efficient state persistence
  persistCriticalState(): Promise<void>;
}
```

### 4. Expo-Specific Constraints
```typescript
// Work within Expo limitations
interface ExpoTradingFeatures {
  // Background updates for price alerts
  scheduleBackgroundNotifications(alerts: PriceAlert[]): Promise<void>;
  
  // Secure storage for trading credentials
  storeSecureCredentials(credentials: TradingCredentials): Promise<void>;
  
  // Biometric authentication for trades
  authenticateTradeWithBiometrics(order: Order): Promise<boolean>;
  
  // Push notifications for market events
  sendRealTimeNotifications(events: MarketEvent[]): Promise<void>;
}
```

---

## Specific Implementation Challenges

### Challenge 1: Real-Time Chart Performance
Build a candlestick chart that:
- Renders 10,000+ data points at 60fps
- Updates in real-time without flickering
- Supports pinch-to-zoom and pan gestures
- Shows technical indicators (moving averages, RSI, MACD)
- Handles different timeframes (1min, 5min, 1hour, 1day)

```typescript
// Your implementation must handle this data volume
const chartData: OHLCV[] = Array.from({length: 10000}, (_, i) => ({
  timestamp: Date.now() - (i * 60000),
  open: Math.random() * 100 + 100,
  high: Math.random() * 100 + 120,
  low: Math.random() * 100 + 80,
  close: Math.random() * 100 + 100,
  volume: Math.random() * 1000000
}));
```

### Challenge 2: Collaborative Features
Implement real-time collaboration where:
- Multiple users can annotate the same chart simultaneously
- Cursor positions are shared in real-time
- Chat messages appear instantly
- Shared watchlists update for all room members
- No conflicts when multiple users make changes

```typescript
// Handle simultaneous updates from multiple users
interface CollaborativeUpdate {
  userId: string;
  timestamp: number;
  type: 'annotation' | 'cursor' | 'watchlist' | 'chat';
  data: any;
}

// Implement operational transforms for conflict resolution
class OperationalTransform {
  transform(localOp: Operation, remoteOp: Operation): Operation[];
  compose(ops: Operation[]): Operation;
  apply(doc: Document, op: Operation): Document;
}
```

### Challenge 3: Complex Financial Calculations
Implement sophisticated trading algorithms:

```typescript
// Technical analysis calculations
class TechnicalAnalysis {
  // Simple Moving Average with configurable periods
  calculateSMA(prices: number[], period: number): number[];
  
  // Exponential Moving Average
  calculateEMA(prices: number[], period: number): number[];
  
  // Relative Strength Index
  calculateRSI(prices: number[], period: number): number[];
  
  // MACD with signal line
  calculateMACD(prices: number[]): {
    macd: number[];
    signal: number[];
    histogram: number[];
  };
  
  // Bollinger Bands
  calculateBollingerBands(prices: number[], period: number, multiplier: number): {
    upper: number[];
    middle: number[];
    lower: number[];
  };
}

// Portfolio risk calculations
class RiskManagement {
  // Value at Risk calculation
  calculateVaR(portfolio: Portfolio, confidence: number): number;
  
  // Portfolio correlation matrix
  calculateCorrelationMatrix(symbols: string[]): number[][];
  
  // Maximum Sharpe ratio portfolio
  optimizePortfolio(expectedReturns: number[], covarianceMatrix: number[][]): number[];
}
```

### Challenge 4: Memory and Performance Management
Handle large datasets efficiently:

```typescript
// Efficient data structures for real-time updates
class CircularBuffer<T> {
  private buffer: T[];
  private head: number = 0;
  private tail: number = 0;
  private size: number = 0;
  
  constructor(private capacity: number) {
    this.buffer = new Array(capacity);
  }
  
  push(item: T): void;
  pop(): T | undefined;
  peek(): T | undefined;
  clear(): void;
  toArray(): T[];
}

// Memory-efficient market data storage
class MarketDataCache {
  private cache: Map<string, CircularBuffer<MarketDataStream>>;
  private maxSize: number = 1000; // per symbol
  
  addData(symbol: string, data: MarketDataStream): void;
  getData(symbol: string, count?: number): MarketDataStream[];
  cleanup(): void; // Remove old data to prevent memory leaks
}
```

---

## Evaluation Criteria

### Code Architecture (30 points)
- Clean separation of concerns
- Efficient data structures
- Proper error handling
- Memory management
- Scalable design patterns

### Performance (25 points)
- Smooth 60fps rendering
- Efficient real-time updates
- Memory usage optimization
- Battery life considerations
- Network efficiency

### Expo Integration (25 points)
- Proper use of Expo APIs
- Background task handling
- Secure storage implementation
- Push notification setup
- Cross-platform compatibility

### Feature Completeness (20 points)
- Working real-time data
- Functional portfolio management
- Collaborative features
- Order management system
- Trading calculations

---

## Bonus Challenges (+10 points each)

1. **Machine Learning Integration**: Implement price prediction using TensorFlow.js
2. **Advanced Animations**: Create smooth transitions and micro-interactions
3. **Accessibility**: Full VoiceOver/TalkBack support for trading features
4. **Offline Capabilities**: Queue trades and sync when reconnected
5. **Testing Suite**: Comprehensive unit and integration tests

---

## Submission Requirements

### 1. Complete Code Implementation
- All TypeScript interfaces implemented
- Working Expo app with all features
- Proper error handling and edge cases
- Clean, documented code

### 2. Architecture Documentation
- System design overview
- State management strategy
- Performance optimization techniques
- Expo-specific implementation details

### 3. Demo Video (5 minutes max)
- Show all features working
- Demonstrate real-time collaboration
- Performance benchmarks
- Explain key technical decisions

### 4. Testing Evidence
- Performance metrics
- Memory usage reports
- Load testing results
- Cross-platform compatibility

---

## Getting Started

### Initial Setup
```bash
npx create-expo-app TradingPlatform --template
cd TradingPlatform
npm install @expo/vector-icons expo-secure-store expo-notifications
```

### Required Dependencies
```json
{
  "dependencies": {
    "expo": "~49.0.0",
    "react": "18.2.0",
    "react-native": "0.72.0",
    "@expo/vector-icons": "^13.0.0",
    "expo-secure-store": "~12.3.1",
    "expo-notifications": "~0.20.1",
    "expo-local-authentication": "~13.4.1",
    "react-native-gesture-handler": "~2.12.0",
    "react-native-reanimated": "~3.3.0"
  }
}
```

### Mock Data Service
```typescript
// Use this for market data simulation
export const mockMarketData = {
  symbols: ['AAPL', 'GOOGL', 'MSFT', 'TSLA', 'AMZN', /* ... 95 more */],
  generateRealTimeData: (symbol: string) => MarketDataStream,
  generateHistoricalData: (symbol: string, days: number) => OHLCV[]
};
```

---

*This challenge tests advanced React Native skills, real-time data handling, complex state management, and production-ready code quality. Focus on creating a working prototype that demonstrates your architectural thinking and problem-solving abilities.*