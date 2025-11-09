# Real-World System Design Examples

## 1. E-commerce Platform (Amazon-like)

### Requirements
- **Functional**: Product catalog, shopping cart, payments, order management
- **Non-Functional**: 100M+ users, 99.9% availability, global scale

### High-Level Architecture

```
[CDN] → [Load Balancer] → [API Gateway] → [Microservices]
                                          ↓
[User Service] [Product Service] [Cart Service] [Order Service] [Payment Service]
     ↓              ↓               ↓              ↓              ↓
[User DB]    [Product Search]   [Redis Cache] [Order DB]   [Payment Gateway]
             [Product DB]
```

### Key Components
- **Product Catalog Service**: Search, recommendations
- **User Management Service**: Authentication, profiles
- **Shopping Cart Service**: Session management
- **Order Management Service**: Order processing workflow
- **Payment Service**: Payment processing and fraud detection
- **Inventory Service**: Stock management
- **Notification Service**: Email/SMS notifications

### Scaling Strategies
- **Database Sharding**: Partition by user ID or geography
- **Caching**: Product details, user sessions
- **CDN**: Static assets, product images
- **Event-Driven**: Order processing workflow
- **Read Replicas**: Product catalog queries

---

## 2. Social Media Platform (Twitter-like)

### Requirements
- **Functional**: Post tweets, follow users, timeline, search
- **Non-Functional**: 500M users, 300K tweets/sec, sub-second response

### High-Level Architecture

```
[Mobile Apps] → [Load Balancer] → [API Gateway] → [Tweet Service]
[Web Client]                                      [User Service]
                                                  [Timeline Service]
                                                  [Search Service]
                                                       ↓
[Message Queue] ← [Fan-out Service] ← [Tweet Processing]
     ↓
[Timeline Generation] → [Timeline Cache] → [Timeline DB]
```

### Key Design Decisions

#### Timeline Generation
**Push Model (Write-Heavy)**
- Pre-generate timelines when tweet is posted
- Fast reads, expensive writes
- Good for users with many followers

**Pull Model (Read-Heavy)**  
- Generate timeline when requested
- Fast writes, expensive reads
- Good for users with few followers

**Hybrid Approach**
- Push for normal users
- Pull for celebrities (millions of followers)

### Components
- **Tweet Service**: Create, store tweets
- **User Graph Service**: Follow relationships
- **Timeline Service**: Generate user timelines
- **Search Service**: Real-time tweet search
- **Media Service**: Handle images/videos
- **Notification Service**: Push notifications

---

## 3. Video Streaming Service (Netflix-like)

### Requirements
- **Functional**: Upload videos, stream content, user preferences
- **Non-Functional**: 200M+ users, 4K streaming, global CDN

### High-Level Architecture

```
[Client Apps] → [CDN (Global)] → [Origin Servers]
                     ↓
[Video Processing Pipeline] → [Multiple Formats/Bitrates]
                     ↓
[Content Metadata Service] → [Recommendation Engine]
                     ↓
[User Service] → [Viewing History] → [Analytics Pipeline]
```

### Video Processing Pipeline
1. **Upload**: Original video file
2. **Transcoding**: Multiple formats (MP4, WebM)
3. **Bitrate Variants**: 480p, 720p, 1080p, 4K
4. **CDN Distribution**: Global content replication
5. **Adaptive Streaming**: Dynamic quality adjustment

### Key Components
- **Content Management**: Video metadata, thumbnails
- **User Management**: Profiles, preferences, subscriptions  
- **Recommendation Engine**: ML-based content suggestions
- **Analytics Service**: View tracking, performance metrics
- **Billing Service**: Subscription management
- **Content Delivery**: Global CDN network

### Optimization Strategies
- **Pre-positioning**: Popular content in regional CDNs
- **Adaptive Bitrate**: Quality based on network conditions
- **P2P Delivery**: Reduce CDN costs for popular content
- **Caching Strategy**: Predictive content caching

---

## 4. Chat Application (WhatsApp-like)

### Requirements
- **Functional**: Send/receive messages, group chat, online status
- **Non-Functional**: 2B+ users, real-time delivery, high availability

### High-Level Architecture

```
[Mobile Clients] → [Load Balancer] → [Connection Servers (WebSocket)]
                                            ↓
[Message Router] → [Message Queue] → [Message Service]
                                            ↓
[User Service] ← [Presence Service] → [Message Storage]
     ↓                                      ↓
[User DB]                            [Message DB (Sharded)]
```

### Real-time Communication
- **WebSocket Connections**: Persistent connections
- **Connection Servers**: Handle client connections
- **Message Routing**: Route messages to online users
- **Message Queue**: Handle offline users
- **Push Notifications**: For offline users

### Data Storage
- **Message Sharding**: Partition by chat ID or user ID
- **Data Replication**: Multi-region for availability
- **Message Encryption**: End-to-end security
- **Media Storage**: Separate storage for images/files

### Scalability Features
- **Connection Pooling**: Efficient connection management
- **Message Batching**: Reduce network calls
- **Presence Optimization**: Heartbeat mechanisms
- **Group Chat Optimization**: Efficient fan-out

---

## 5. Payment Processing System

### Requirements
- **Functional**: Process payments, fraud detection, reconciliation
- **Non-Functional**: 99.99% availability, PCI compliance, sub-second processing

### High-Level Architecture

```
[Merchant APIs] → [API Gateway] → [Payment Service]
                                        ↓
[Fraud Detection] ← [Event Stream] ← [Transaction Processor]
        ↓                                ↓
[ML Models]                       [Payment Gateway]
                                        ↓
[Reconciliation Service] ← [Transaction DB] → [Audit Service]
```

### Security Measures
- **Encryption**: All data encrypted at rest and in transit
- **Tokenization**: Replace sensitive data with tokens
- **PCI Compliance**: Follow payment industry standards
- **Fraud Detection**: Real-time ML-based detection
- **Audit Logging**: Complete transaction history

### Key Components
- **Payment Processor**: Core transaction handling
- **Fraud Detection Engine**: Risk scoring and blocking
- **Reconciliation Service**: Balance verification
- **Notification Service**: Transaction alerts
- **Reporting Service**: Financial analytics
- **Compliance Service**: Regulatory requirements

---

## Common Patterns Across Examples

### Scalability Patterns
- **Horizontal Scaling**: Add more servers
- **Database Sharding**: Partition data across databases
- **Caching**: Multiple layers of caching
- **CDN**: Geographic content distribution
- **Load Balancing**: Distribute traffic

### Reliability Patterns  
- **Redundancy**: Multiple instances and regions
- **Circuit Breakers**: Prevent cascade failures
- **Graceful Degradation**: Reduced functionality during issues
- **Health Checks**: Monitor service health
- **Disaster Recovery**: Backup and recovery procedures

### Performance Patterns
- **Asynchronous Processing**: Non-blocking operations
- **Event-Driven Architecture**: Loose coupling
- **Data Denormalization**: Optimize for read performance
- **Connection Pooling**: Efficient resource usage
- **Batch Processing**: Process data in batches