# Rabbit - Knowledge Graph Browser

Rabbit is an intelligent knowledge exploration application that helps you discover, organize, and visualize information through an interactive knowledge graph. Built with Next.js, Electron, and powered by Google Gemini AI, Rabbit transforms web browsing into a structured learning experience.

## 🎯 Overview

Rabbit combines web search, AI-powered concept extraction, and interactive visualization to create a "den" of knowledge around any topic. As you explore the web, Rabbit automatically extracts concepts, identifies relationships, and builds a visual knowledge graph that grows with your research.

## 🏗️ Architecture

This is a monorepo containing three main components:

- **Frontend** (`frontend/`): Next.js 15 application with React 19, providing the search interface and knowledge graph visualization
- **Backend** (`backend/`): Express.js API server handling search, AI processing, and knowledge graph management
- **Browser** (`browser/`): Electron application providing a custom browser experience with keyboard shortcuts and seamless integration

## ✨ Features

### Core Functionality

- **Intelligent Search**: Search the web and automatically extract key concepts from results
- **Knowledge Graph**: Visualize relationships between concepts in an interactive graph
- **Concept Extraction**: AI-powered extraction of key concepts from web pages
- **Den Management**: Organize information into hierarchical "den" structures (bigDaddyNode and babyNode)
- **Hop Sessions**: Navigate through search results with keyboard shortcuts
- **Burrow Sessions**: Deep dive into specific concepts with dedicated navigation sessions
- **Page Proxy**: Browse web pages through a proxy that integrates with the knowledge graph

### Keyboard Shortcuts

- **Alt+G**: Toggle between graph view and browser
- **Alt+W**: Reset and return to home
- **Ctrl+D**: Send current page to the knowledge den
- **Ctrl+Left/Right**: Navigate through hop/burrow sessions
- **Alt+B**: Go back to home from graph view

## 🚀 Getting Started

### Prerequisites

- Node.js 18+ and npm 11.4.1+
- Google Gemini API key (for AI features)
- Either:
  - Google Custom Search API key + Custom Search Engine ID, OR
  - Serper API key (for web search)

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd hackrice25
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the root directory:
```env
# Required: Google Gemini API Key for AI features
GEMINI_API_KEY=your_gemini_api_key_here

# Required: Choose one search provider
# Option 1: Google Custom Search
GOOGLE_API_KEY=your_google_api_key_here
GOOGLE_CSE_ID=your_custom_search_engine_id_here

# Option 2: Serper API
SERPER_API_KEY=your_serper_api_key_here

# Optional: Backend port (defaults to 4000)
PORT=4000
```

### Running the Application

#### Development Mode

Run all services in development mode:
```bash
npm run dev
```

This starts:
- Frontend on `http://localhost:3000`
- Backend on `http://localhost:4000`
- Electron browser application

#### Individual Services

**Backend only:**
```bash
cd backend
npm run dev
```

**Frontend only:**
```bash
cd frontend
npm run dev
```

**Browser only:**
```bash
cd browser
npm start
```

### Building for Production

Build all packages:
```bash
npm run build
```

Build and package Electron app:
```bash
npm run package
npm run make
```

## 📁 Project Structure

```
hackrice25/
├── backend/              # Express.js API server
│   ├── src/
│   │   ├── index.ts     # Main server entry point
│   │   ├── services/     # Business logic services
│   │   │   ├── search.ts           # Web search functionality
│   │   │   ├── get_concepts.ts     # Concept extraction
│   │   │   ├── get_answer.ts       # Answer generation
│   │   │   ├── burrow.ts           # Burrow session management
│   │   │   ├── hop.ts              # Hop session management
│   │   │   ├── graph_generator.ts  # Knowledge graph generation
│   │   │   └── send_toDen.ts       # Add pages to den
│   │   └── types/
│   │       └── den.ts    # TypeScript type definitions
│   └── package.json
│
├── frontend/             # Next.js application
│   ├── src/
│   │   ├── app/          # Next.js app router pages
│   │   │   ├── page.tsx           # Home/search page
│   │   │   ├── graph/             # Graph visualization page
│   │   │   └── search/            # Search results page
│   │   ├── components/            # React components
│   │   │   ├── KnowledgeGraph.tsx # Main graph component
│   │   │   ├── search_bar.tsx     # Search interface
│   │   │   └── ...
│   │   └── store.ts      # Zustand state management
│   └── package.json
│
├── browser/              # Electron application
│   ├── index.js         # Main Electron process
│   ├── index.html       # Browser window HTML
│   ├── renderer.js      # Renderer process script
│   └── package.json
│
├── package.json         # Root package.json (monorepo config)
└── turbo.json          # Turbo build configuration
```

## 🔧 API Endpoints

### Search & Navigation

- `GET /search?query=<query>` - Perform web search and create central node
- `GET /central-node-state` - Get current central node state
- `POST /update-central-node` - Update the central node
- `POST /clear-memory` - Clear all memory and reset state

### Hop Sessions

- `POST /hop` - Create a new hop session
- `GET /hop/:sessionId` - Get hop session state
- `POST /hop/:sessionId/navigate` - Navigate (next/prev) in hop session
- `GET /hop/:sessionId/current` - Get current page in hop session
- `GET /hop/:sessionId/pages` - Get all pages in hop session
- `DELETE /hop/:sessionId` - Delete hop session

### Burrow Sessions

- `POST /burrow-into-child` - Initiate burrow into a child node
- `GET /burrow/:childNodeId` - Get burrow session state
- `POST /burrow/:childNodeId/navigate` - Navigate in burrow session
- `POST /burrow/:childNodeId/add-page` - Add page to burrowed node
- `GET /burrow/:childNodeId/current` - Get current page
- `GET /burrow/:childNodeId/pages` - Get all pages
- `DELETE /burrow/:childNodeId` - Delete burrow session

### Knowledge Graph

- `GET /generate-graph` - Generate full knowledge graph
- `GET /generate-graph-preview?maxDepth=<depth>` - Generate preview graph

### Den Operations

- `POST /send-to-den` - Send current page to the den (Ctrl+D)
- `POST /get-concepts` - Extract concepts from a URL
- `POST /get-answer` - Generate answer from URLs and concepts
- `POST /simplify-concepts` - Remove duplicate concepts
- `POST /get-comparison-score` - Calculate similarity between strings

### Proxy

- `GET /proxy?url=<url>` - Proxy web pages for embedding

## 🧠 How It Works

### Knowledge Graph Structure

Rabbit organizes information into a hierarchical structure:

- **bigDaddyNode**: The root node representing a search query
  - Contains: query, pages, denPages, conceptList, children, answer
- **babyNode**: Child nodes representing specific concepts
  - Contains: title, pages, conceptList, children, parent reference

### Workflow

1. **Search**: User enters a query → Backend searches web → Creates central bigDaddyNode
2. **Concept Extraction**: AI extracts key concepts from search results
3. **Graph Building**: Concepts are organized into child nodes with relationships
4. **Exploration**: User can:
   - View the knowledge graph
   - Navigate through pages (Hop)
   - Deep dive into concepts (Burrow)
   - Add pages to the den (Ctrl+D)
5. **Visualization**: Interactive graph shows relationships and allows navigation

### AI Integration

Rabbit uses Google Gemini 1.5 Flash for:
- **Concept Extraction**: Identifying key concepts from web pages
- **Answer Generation**: Synthesizing answers from multiple sources
- **Similarity Scoring**: Comparing concepts to build relationships
- **Concept Simplification**: Removing duplicates and similar concepts

## 🛠️ Development

### Tech Stack

- **Frontend**: Next.js 15, React 19, TypeScript, Tailwind CSS, ReactFlow, Zustand
- **Backend**: Express.js, TypeScript, Node.js
- **Browser**: Electron 38
- **Build System**: Turbo (monorepo)
- **AI**: Google Gemini 1.5 Flash
- **Search**: Google Custom Search API or Serper API

### Scripts

- `npm run dev` - Start all services in development mode
- `npm run build` - Build all packages
- `npm run lint` - Lint all packages
- `npm run test` - Run tests (if configured)
- `npm run clean` - Clean build artifacts
- `npm start` - Start Electron app (from root)
- `npm run package` - Package Electron app
- `npm run make` - Create distributables

### Code Style

- TypeScript strict mode enabled
- ESLint configured for Next.js
- Consistent formatting across packages

## 📝 Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `GEMINI_API_KEY` | Yes | Google Gemini API key for AI features |
| `GOOGLE_API_KEY` | Yes* | Google Custom Search API key |
| `GOOGLE_CSE_ID` | Yes* | Google Custom Search Engine ID |
| `SERPER_API_KEY` | Yes* | Alternative to Google Custom Search |
| `PORT` | No | Backend server port (default: 4000) |

*Either Google Custom Search (API key + CSE ID) or Serper API key is required.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

[Add your license information here]

## 🙏 Acknowledgments

- Google Gemini AI for concept extraction and analysis
- ReactFlow for graph visualization
- Electron for cross-platform desktop application
- Next.js team for the excellent framework

---

**Note**: This project was built for HackRice 25. For questions or issues, please open an issue on GitHub.

