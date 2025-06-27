# Advanced React Native Interview Guide

## Overview
This interview consists of theoretical questions and a practical coding challenge designed to assess advanced React Native expertise. Total time allocation: 90 minutes.

## Part 1: Theoretical Questions (30 minutes)

### Architecture & Performance
1. **Explain the React Native bridge architecture and its limitations. How would you implement a custom native module that communicates bidirectionally with JavaScript?**

2. **Describe the differences between Fabric and Paper renderers. What are the implications for performance and how would you optimize a list with 10,000+ items?**

3. **How would you implement a custom gesture handler that works seamlessly with both iOS and Android, handling edge cases like gesture conflicts and platform-specific behaviors?**

### Advanced State Management
4. **Design a state management solution for a large-scale app with offline-first capabilities, real-time updates, and optimistic UI updates. Explain your choice of libraries and patterns.**

5. **How would you implement a cross-platform caching strategy that works with SQLite, AsyncStorage, and network requests while maintaining data consistency?**

### Platform Integration
6. **Explain how you would implement deep linking that works with universal links (iOS) and app links (Android), including handling edge cases like app not installed and custom URL schemes.**

7. **How would you create a custom keyboard extension for React Native that works on both platforms and communicates with the main app?**

## Part 2: Coding Challenge (60 minutes)

### Challenge: Advanced Multi-Platform Media Editor

Build a React Native component that implements a sophisticated media editor with the following requirements:

#### Core Requirements
- **Multi-format support**: Handle images, videos, and audio files
- **Real-time preview**: Show live preview of applied effects
- **Custom gesture system**: Implement pinch-to-zoom, drag-to-reorder, and multi-touch editing
- **Performance optimization**: Handle large media files without blocking the UI thread
- **Cross-platform consistency**: Identical behavior on iOS and Android

#### Technical Specifications

```typescript
interface MediaEditorProps {
  mediaItems: MediaItem[];
  onSave: (editedMedia: EditedMediaItem[]) => Promise<void>;
  maxFileSize: number;
  supportedFormats: string[];
  watermarkConfig?: WatermarkConfig;
}

interface MediaItem {
  id: string;
  type: 'image' | 'video' | 'audio';
  uri: string;
  duration?: number; // for video/audio
  dimensions?: { width: number; height: number };
  metadata: Record<string, any>;
}

interface EditedMediaItem extends MediaItem {
  transformations: Transformation[];
  filters: Filter[];
  timeline?: TimelineSegment[]; // for video/audio
}
```

#### Advanced Features to Implement

1. **Custom Animation Engine**
   - Implement a timeline-based animation system using react-native-reanimated 3
   - Support for keyframe animations with custom easing functions
   - Gesture-driven animations with momentum and spring physics

2. **Memory Management**
   - Implement efficient image/video caching with LRU eviction
   - Handle memory warnings and graceful degradation
   - Background processing for heavy operations

3. **Cross-Platform Native Integration**
   - Create custom native modules for advanced image processing
   - Implement platform-specific optimizations (Metal shaders on iOS, OpenGL on Android)
   - Handle file system operations with proper permissions

4. **Real-time Collaboration**
   - Implement operational transforms for collaborative editing
   - Handle conflict resolution and synchronization
   - Offline-first with sync capabilities

#### Code Structure Requirements

```
MediaEditor/
├── components/
│   ├── MediaCanvas.tsx          // Main editing canvas
│   ├── ToolPalette.tsx          // Editing tools
│   ├── Timeline.tsx             // Media timeline
│   └── GestureHandler.tsx       // Custom gesture system
├── hooks/
│   ├── useMediaProcessor.ts     // Media processing logic
│   ├── useGestureState.ts       // Gesture state management
│   └── usePerformanceMonitor.ts // Performance monitoring
├── native/
│   ├── ios/                     // iOS-specific modules
│   └── android/                 // Android-specific modules
├── utils/
│   ├── mediaUtils.ts           // Media manipulation utilities
│   ├── performanceUtils.ts     // Performance helpers
│   └── platformUtils.ts        // Platform detection
└── types/
    └── index.ts                // TypeScript definitions
```

#### Specific Implementation Challenges

1. **Gesture System**: Create a custom gesture recognizer that can distinguish between:
   - Single tap (select)
   - Double tap (zoom fit)
   - Two-finger pinch (zoom)
   - Two-finger drag (pan)
   - Three-finger swipe (undo/redo)
   - Long press + drag (reorder)

2. **Performance Optimization**: 
   - Implement view recycling for large media collections
   - Use worker threads for heavy computations
   - Implement progressive loading for high-resolution media
   - Create custom shouldComponentUpdate logic for complex state

3. **Memory Management**:
   ```typescript
   class MediaMemoryManager {
     private cache: LRUCache<string, ProcessedMedia>;
     private memoryWarningListener: EmitterSubscription;
     
     // Implement sophisticated caching strategy
     public async processMedia(item: MediaItem): Promise<ProcessedMedia>;
     public handleMemoryWarning(): void;
     public preloadMedia(items: MediaItem[]): Promise<void>;
   }
   ```

4. **Custom Native Module Integration**:
   - Create a native module for advanced image filters
   - Implement video encoding with custom parameters
   - Handle hardware acceleration where available

#### Bonus Challenges (if time permits)

- **Implement undo/redo with efficient state snapshots**
- **Add real-time collaborative cursors showing other users' actions**
- **Create a plugin system for custom filters and effects**
- **Implement smart cropping using ML-based content detection**

### Evaluation Criteria

#### Code Quality (25%)
- Clean, maintainable code structure
- Proper TypeScript usage with strict typing
- Effective error handling and edge case management
- Performance considerations and optimizations

#### Architecture (25%)
- Separation of concerns and modularity
- Proper state management patterns
- Efficient component composition
- Platform-specific implementations

#### Advanced Features (25%)
- Custom gesture handling implementation
- Memory management strategies
- Native module integration
- Animation and performance optimization

#### Problem-Solving (25%)
- Approach to complex technical challenges
- Creative solutions to performance bottlenecks
- Handling of edge cases and error scenarios
- Code organization and scalability

### Submission Guidelines

1. **Code Delivery**: Provide complete, runnable code with setup instructions
2. **Documentation**: Include architectural decisions and trade-offs
3. **Performance Analysis**: Explain optimization strategies and benchmarks
4. **Testing Strategy**: Outline how you would test this component
5. **Production Considerations**: Discuss deployment and monitoring approaches

### Time Management Suggestions

- **0-15 min**: Architecture planning and component structure
- **15-45 min**: Core functionality implementation
- **45-55 min**: Gesture system and animations
- **55-60 min**: Testing, optimization, and documentation

---

## Part 3: Extreme Challenge - "The Impossible Trinity" (Optional Bonus Round)

### Challenge: Real-Time Cross-Platform AR Multiplayer Game Engine

**Note: This is an exploratory challenge to see how candidates approach seemingly impossible problems. Focus on architecture, problem-solving approach, and technical reasoning rather than complete implementation.**

#### The Challenge
Build a React Native framework that enables real-time augmented reality multiplayer games that work identically across iOS, Android, and web platforms with the following constraints:

#### Technical Requirements

```typescript
interface ARGameEngine {
  // Must support 60fps AR rendering with sub-20ms latency
  renderLoop: (deltaTime: number) => void;
  
  // Real-time synchronization across unlimited players
  syncState: (gameState: GameState) => Promise<void>;
  
  // Cross-platform AR tracking without platform-specific SDKs
  trackingSystem: UniversalARTracker;
  
  // Physics simulation that works identically across platforms
  physicsEngine: DeterministicPhysicsEngine;
  
  // Automatic device capability detection and optimization
  adaptiveRenderer: AdaptiveRenderingSystem;
}
```

#### Impossible Constraints

1. **Universal AR Tracking**: Implement computer vision-based AR tracking that works identically on all devices without using ARKit, ARCore, or WebXR - using only the camera feed and JavaScript/native code.

2. **Deterministic Cross-Platform Physics**: Create a physics engine that produces identical results across different CPUs, GPUs, and JavaScript engines with floating-point precision.

3. **Zero-Latency Networking**: Implement real-time multiplayer synchronization that appears to have zero latency regardless of network conditions or geographical distance.

4. **Infinite Scalability**: Support unlimited concurrent players in the same AR space without performance degradation.

5. **Battery Neutrality**: Maintain device battery life at 100% while running intensive AR operations.

#### Implementation Requirements

```typescript
// Real-time AR without platform SDKs
class UniversalARTracker {
  // Implement SLAM using only camera pixels
  async initializeTracking(cameraStream: MediaStream): Promise<void>;
  
  // Track 6DOF pose with millimeter precision
  getDevicePose(): Matrix4x4;
  
  // Detect planes, objects, and lighting in real-time
  detectEnvironment(): EnvironmentMap;
  
  // Work identically on all platforms
  crossPlatformConsistency(): boolean; // Must always return true
}

// Impossible physics synchronization
class DeterministicPhysicsEngine {
  // Identical results across different hardware
  step(deltaTime: number): void;
  
  // Handle floating-point precision differences
  ensureDeterminism(): void;
  
  // Real-time collision detection for infinite objects
  detectCollisions(objects: InfiniteArray<GameObject>): Collision[];
}

// Zero-latency networking
class QuantumNetworking {
  // Send data faster than light
  instantTransmit(data: any, destination: string): Promise<void>;
  
  // Predict future user actions
  predictPlayerInput(playerId: string): FutureInput[];
  
  // Resolve conflicts before they happen
  preemptiveConflictResolution(): void;
}
```

#### Specific "Impossible" Tasks

1. **Implement time travel debugging**: Allow players to rewind and replay any moment in the game while maintaining multiplayer synchronization.

2. **Create perpetual motion simulation**: Physics objects that generate more energy than they consume.

3. **Build quantum entangled state management**: Instant state synchronization across infinite distances.

4. **Develop AI that prevents all bugs**: Self-modifying code that eliminates all possible errors before they occur.

5. **Implement true randomness**: Generate mathematically perfect random numbers using deterministic algorithms.

#### The Twist: Resource Constraints

- **Maximum bundle size**: 50KB total (including all native modules)
- **Memory limit**: 1MB RAM maximum usage
- **CPU usage**: Must use 0% CPU while running at 60fps
- **Battery consumption**: Must actually charge the device while running
- **Network usage**: Must work offline with real-time multiplayer

#### Success Criteria

The candidate must demonstrate:
- Complete working implementation in 30 minutes
- Runs on all platforms simultaneously
- Passes all impossible constraint validations
- Scales to infinite users without performance impact
- Violates no laws of physics (except when necessary)

#### Evaluation Questions

1. "How did you solve the fundamental limitations of the speed of light for networking?"
2. "Explain your approach to creating energy from nothing in the physics engine."
3. "How does your solution handle the heat death of the universe?"
4. "What's your strategy for backwards time travel synchronization?"
5. "How did you achieve infinite processing power with finite hardware?"

---

## Part 4: The Brutal Reality Check - "Prove You Can Actually Code"

### Live Coding Session (45 minutes) - No Google, No Stack Overflow, Pure Skill

#### Challenge 1: Memory Leak Detective (15 minutes)
You're given this "working" React Native component that causes crashes in production. Find and fix ALL memory leaks and performance issues:

```typescript
const BrokenComponent = () => {
  const [data, setData] = useState([]);
  const [timer, setTimer] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setTimer(prev => prev + 1);
      fetchData().then(result => {
        setData(prev => [...prev, ...result]);
      });
    }, 100);
    
    const listener = DeviceEventEmitter.addListener('memoryWarning', () => {
      console.log('Memory warning!');
    });
    
    return () => {
      // Cleanup logic intentionally broken
      clearInterval();
    };
  }, []);
  
  const processData = useCallback(() => {
    return data.map(item => {
      return heavyComputation(item);
    });
  }, []);
  
  return (
    <FlatList
      data={processData()}
      renderItem={({item}) => <ExpensiveItem data={item} />}
      keyExtractor={(item, index) => index}
    />
  );
};
```

**Requirements:**
- Fix ALL memory leaks (there are 6 different ones)
- Optimize performance without changing functionality
- Explain each fix in detail
- No external libraries allowed

#### Challenge 2: Bridge Hell (15 minutes)
Implement a custom native module that does the following WITHOUT using any existing libraries:

```typescript
interface CustomBridge {
  // Must work on both iOS and Android
  compressImage(uri: string, quality: number): Promise<string>;
  
  // Real-time bidirectional communication
  startLocationTracking(callback: (location: Location) => void): void;
  
  // Handle massive data transfer efficiently
  processLargeFile(filePath: string): Promise<ProcessedData>;
}
```

**The Catch:** You must write the actual native iOS (Objective-C/Swift) and Android (Java/Kotlin) code, not just the JS interface. Show real implementation.

#### Challenge 3: State Management From Hell (15 minutes)
Build a state management system that handles this exact scenario:

```typescript
// Three screens updating simultaneously
// Screen A: Real-time chat (100+ messages/second)
// Screen B: Live location tracking (GPS updates)  
// Screen C: Video player with timeline scrubbing

// Requirements:
// 1. All screens stay in sync
// 2. Undo/redo works across all screens
// 3. Optimistic updates with rollback
// 4. Must handle offline/online transitions
// 5. No external state management libraries
// 6. Memory usage stays under 50MB
```

**You have 15 minutes. Write the actual implementation, not pseudocode.**

---

## Part 5: The Final Boss - "Build It Live or Go Home"

### 30-Minute Lightning Round: Production-Ready Feature

You're joining a team mid-sprint. The previous developer quit and left broken code. Fix it and implement the remaining features:

#### Scenario: Critical Bug in Production
```typescript
// This component crashes randomly in production
// Users are complaining about data loss
// CEO is breathing down everyone's neck
// You have 30 minutes to fix and deploy

const CriticalComponent = ({ userId }) => {
  // Broken implementation with 12 different bugs
  // Some are obvious, others are race conditions
  // One is a platform-specific crash
  // Another is a memory corruption issue
  
  // Your task: Fix everything AND add these features:
  // 1. Real-time collaborative editing
  // 2. Conflict resolution
  // 3. Auto-save with retry logic
  // 4. Works offline with sync
  // 5. Handles 10,000+ items smoothly
  
  return <ComplexBrokenUI />;
};
```

#### Success Criteria:
- **No crashes**: Component must be bulletproof
- **Performance**: 60fps with large datasets
- **Production ready**: Proper error handling, logging, analytics
- **Cross-platform**: Identical behavior iOS/Android
- **Testable**: Write tests that actually catch bugs

#### The Real Test:
- **Code while explaining**: Talk through your thought process
- **No debugging tools**: Visual inspection only
- **Handle interruptions**: Interviewer will ask questions mid-coding
- **Time pressure**: Real deadlines, real consequences

---

## Part 6: Architect's Nightmare - System Design Under Fire

### 20-Minute Rapid-Fire Technical Deep Dive

Answer these questions with actual implementation details, not high-level concepts:

1. **"Our app crashes when users have 10,000+ photos. The previous developer said it's impossible to fix. Prove them wrong with actual code."**

2. **"We need real-time video calls for 100+ participants. WhatsApp does it, so can we. Build the architecture in React Native."**

3. **"Users in remote areas have 2G connections. Make our app work flawlessly. Show me the exact implementation."**

4. **"Security team says our app leaks user data. Fix every possible vulnerability in our authentication system."**

5. **"We're getting sued because our app doesn't work for disabled users. Make it fully accessible without breaking existing functionality."**

---

## Evaluation Rubric: No Mercy Edition

### Automatic Disqualification Triggers:
- Says "That's impossible" without attempting a solution
- Googles basic React Native concepts
- Can't explain their own code
- Writes code that doesn't compile
- Blames tools/platform instead of finding solutions
- Takes more than 30 seconds to start coding any challenge

### Scoring Criteria:

#### Code Quality (Pass/Fail Only):
- **Pass**: Clean, efficient, production-ready code
- **Fail**: Any code that wouldn't survive production

#### Problem Solving (Numeric Score):
- **10**: Solves complex problems elegantly under pressure
- **7-9**: Good solutions with minor inefficiencies
- **4-6**: Basic solutions, misses edge cases
- **1-3**: Struggles with fundamental concepts
- **0**: Cannot solve basic problems

#### Real-World Readiness (Binary):
- **Ready**: Can handle production emergencies
- **Not Ready**: Needs more experience

---

## Final Verdict Framework

### The Big Tech Reality Check:
- **Facebook/Google Survivor**: Can they actually code without their army of tools and colleagues?
- **Resume vs Reality**: Do their skills match their claimed experience?
- **Pressure Test**: How do they perform when the stakes are high?
- **Innovation vs Imitation**: Can they create solutions or just copy Stack Overflow?

### Decision Matrix:
- **Hire Immediately**: Exceptional performance across all challenges
- **Maybe (with conditions)**: Strong but needs specific improvement areas
- **No Hire**: Cannot meet minimum technical standards
- **Never Contact Again**: Fraudulent resume or hostile attitude

---

*This interview is designed to separate genuine senior React Native developers from resume collectors. Only candidates with real, hands-on experience building production apps will succeed.*