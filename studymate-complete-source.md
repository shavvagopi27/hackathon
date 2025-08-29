# StudyMate AI Academic Assistant - Complete Source Code

## Project Structure
```
studymate/
├── package.json
├── vite.config.ts
├── tailwind.config.js
├── postcss.config.js
├── tsconfig.json
├── tsconfig.app.json
├── tsconfig.node.json
├── eslint.config.js
├── index.html
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── index.css
│   ├── vite-env.d.ts
│   ├── types/
│   │   └── index.ts
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── DocumentLibrary.tsx
│   │   ├── ChatInterface.tsx
│   │   └── DocumentUpload.tsx
│   └── utils/
│       └── mockData.ts
```

## Configuration Files

### package.json
```json
{
  "name": "vite-react-typescript-starter",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint .",
    "preview": "vite preview"
  },
  "dependencies": {
    "lucide-react": "^0.344.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1"
  },
  "devDependencies": {
    "@eslint/js": "^9.9.1",
    "@types/react": "^18.3.5",
    "@types/react-dom": "^18.3.0",
    "@vitejs/plugin-react": "^4.3.1",
    "autoprefixer": "^10.4.18",
    "eslint": "^9.9.1",
    "eslint-plugin-react-hooks": "^5.1.0-rc.0",
    "eslint-plugin-react-refresh": "^0.4.11",
    "globals": "^15.9.0",
    "postcss": "^8.4.35",
    "tailwindcss": "^3.4.1",
    "typescript": "^5.5.3",
    "typescript-eslint": "^8.3.0",
    "vite": "^5.4.2"
  }
}
```

### vite.config.ts
```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  optimizeDeps: {
    exclude: ['lucide-react'],
  },
});
```

### tailwind.config.js
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### postcss.config.js
```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
```

### tsconfig.json
```json
{
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ]
}
```

### tsconfig.app.json
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,
    "jsx": "react-jsx",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src"]
}
```

### tsconfig.node.json
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2023"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["vite.config.ts"]
}
```

### eslint.config.js
```javascript
import js from '@eslint/js';
import globals from 'globals';
import reactHooks from 'eslint-plugin-react-hooks';
import reactRefresh from 'eslint-plugin-react-refresh';
import tseslint from 'typescript-eslint';

export default tseslint.config(
  { ignores: ['dist'] },
  {
    extends: [js.configs.recommended, ...tseslint.configs.recommended],
    files: ['**/*.{ts,tsx}'],
    languageOptions: {
      ecmaVersion: 2020,
      globals: globals.browser,
    },
    plugins: {
      'react-hooks': reactHooks,
      'react-refresh': reactRefresh,
    },
    rules: {
      ...reactHooks.configs.recommended.rules,
      'react-refresh/only-export-components': [
        'warn',
        { allowConstantExport: true },
      ],
    },
  }
);
```

### index.html
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>StudyMate AI Academic Assistant</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

## Source Code

### src/main.tsx
```typescript
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import App from './App.tsx';
import './index.css';

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

### src/index.css
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### src/vite-env.d.ts
```typescript
/// <reference types="vite/client" />
```

### src/types/index.ts
```typescript
export interface Document {
  id: string;
  name: string;
  size: string;
  uploadDate: string;
  pageCount: number;
  type: 'pdf';
}

export interface ChatMessage {
  id: string;
  type: 'user' | 'assistant';
  content: string;
  timestamp: string;
  sources?: DocumentReference[];
}

export interface DocumentReference {
  documentId: string;
  documentName: string;
  pageNumber: number;
  snippet: string;
}
```

### src/App.tsx
```typescript
import React, { useState } from 'react';
import { Header } from './components/Header';
import { DocumentLibrary } from './components/DocumentLibrary';
import { ChatInterface } from './components/ChatInterface';
import { DocumentUpload } from './components/DocumentUpload';
import { Document, ChatMessage } from './types';
import { mockDocuments, generateMockResponse } from './utils/mockData';

function App() {
  const [documents, setDocuments] = useState<Document[]>(mockDocuments);
  const [selectedDocuments, setSelectedDocuments] = useState<Document[]>([]);
  const [chatMessages, setChatMessages] = useState<ChatMessage[]>([]);
  const [activeView, setActiveView] = useState<'upload' | 'library' | 'chat'>('upload');
  const [isTyping, setIsTyping] = useState(false);

  const handleDocumentUpload = (newDocuments: Document[]) => {
    setDocuments(prev => [...prev, ...newDocuments]);
    if (newDocuments.length > 0) {
      setActiveView('library');
    }
  };

  const handleDocumentSelect = (document: Document) => {
    setSelectedDocuments(prev => {
      const isSelected = prev.some(d => d.id === document.id);
      if (isSelected) {
        return prev.filter(d => d.id !== document.id);
      }
      return [...prev, document];
    });
  };

  const handleStartChat = () => {
    if (selectedDocuments.length > 0) {
      setActiveView('chat');
    }
  };

  const handleSendMessage = (message: string) => {
    const userMessage: ChatMessage = {
      id: Date.now().toString(),
      type: 'user',
      content: message,
      timestamp: new Date().toISOString()
    };
    
    setChatMessages(prev => [...prev, userMessage]);
    setIsTyping(true);

    // Simulate AI response with realistic delay
    setTimeout(() => {
      const aiResponse = generateMockResponse(message);
      setChatMessages(prev => [...prev, aiResponse]);
      setIsTyping(false);
    }, 1500 + Math.random() * 1000);
  };

  return (
    <div className="min-h-screen bg-gray-50">
      <Header />
      
      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        {activeView === 'upload' && (
          <DocumentUpload 
            onUpload={handleDocumentUpload}
            onViewLibrary={() => setActiveView('library')}
            documents={documents}
          />
        )}
        
        {activeView === 'library' && (
          <DocumentLibrary
            documents={documents}
            selectedDocuments={selectedDocuments}
            onDocumentSelect={handleDocumentSelect}
            onStartChat={handleStartChat}
            onUploadMore={() => setActiveView('upload')}
          />
        )}
        
        {activeView === 'chat' && (
          <ChatInterface
            selectedDocuments={selectedDocuments}
            messages={chatMessages}
            isTyping={isTyping}
            onSendMessage={handleSendMessage}
            onBackToLibrary={() => setActiveView('library')}
          />
        )}
      </main>
    </div>
  );
}

export default App;
```

### src/components/Header.tsx
```typescript
import React from 'react';
import { BookOpen, Brain } from 'lucide-react';

export const Header: React.FC = () => {
  return (
    <header className="bg-white shadow-sm border-b border-gray-200">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div className="flex justify-between items-center py-4">
          <div className="flex items-center space-x-3">
            <div className="flex items-center space-x-2 bg-blue-600 text-white px-3 py-2 rounded-lg">
              <Brain className="h-6 w-6" />
              <BookOpen className="h-5 w-5" />
            </div>
            <div>
              <h1 className="text-2xl font-bold text-gray-900">StudyMate</h1>
              <p className="text-sm text-gray-600">AI-Powered Academic Assistant</p>
            </div>
          </div>
          <div className="hidden sm:block">
            <div className="flex items-center space-x-4 text-sm text-gray-600">
              <span className="flex items-center space-x-1">
                <div className="w-2 h-2 bg-emerald-400 rounded-full"></div>
                <span>Ready to help</span>
              </span>
            </div>
          </div>
        </div>
      </div>
    </header>
  );
};
```

### src/components/DocumentUpload.tsx
```typescript
import React, { useState, useCallback } from 'react';
import { Upload, FileText, BookOpen, ArrowRight, Check } from 'lucide-react';
import { Document } from '../types';

interface DocumentUploadProps {
  onUpload: (documents: Document[]) => void;
  onViewLibrary: () => void;
  documents: Document[];
}

export const DocumentUpload: React.FC<DocumentUploadProps> = ({ onUpload, onViewLibrary, documents }) => {
  const [isDragging, setIsDragging] = useState(false);
  const [uploadProgress, setUploadProgress] = useState<number | null>(null);
  const [uploadComplete, setUploadComplete] = useState(false);

  const handleDragOver = useCallback((e: React.DragEvent) => {
    e.preventDefault();
    setIsDragging(true);
  }, []);

  const handleDragLeave = useCallback((e: React.DragEvent) => {
    e.preventDefault();
    setIsDragging(false);
  }, []);

  const simulateUpload = (files: FileList) => {
    setUploadProgress(0);
    const interval = setInterval(() => {
      setUploadProgress(prev => {
        if (prev === null || prev >= 100) {
          clearInterval(interval);
          setUploadComplete(true);
          setTimeout(() => {
            const newDocuments: Document[] = Array.from(files).map((file, index) => ({
              id: (Date.now() + index).toString(),
              name: file.name,
              size: `${(file.size / (1024 * 1024)).toFixed(1)} MB`,
              uploadDate: new Date().toISOString().split('T')[0],
              pageCount: Math.floor(Math.random() * 200) + 50,
              type: 'pdf' as const
            }));
            onUpload(newDocuments);
            setUploadProgress(null);
            setUploadComplete(false);
          }, 1000);
          return 100;
        }
        return prev + 10;
      });
    }, 200);
  };

  const handleDrop = useCallback((e: React.DragEvent) => {
    e.preventDefault();
    setIsDragging(false);
    const files = e.dataTransfer.files;
    if (files.length > 0) {
      simulateUpload(files);
    }
  }, [onUpload]);

  const handleFileInput = useCallback((e: React.ChangeEvent<HTMLInputElement>) => {
    const files = e.target.files;
    if (files && files.length > 0) {
      simulateUpload(files);
    }
  }, [onUpload]);

  return (
    <div className="max-w-4xl mx-auto">
      <div className="text-center mb-12">
        <h2 className="text-4xl font-bold text-gray-900 mb-4">
          Transform Your Study Materials
        </h2>
        <p className="text-xl text-gray-600 max-w-2xl mx-auto leading-relaxed">
          Upload your PDFs, textbooks, and research papers to start having intelligent conversations with your study materials
        </p>
      </div>

      <div className="grid md:grid-cols-2 gap-8 mb-12">
        <div className="bg-white rounded-xl p-8 shadow-sm border border-gray-200 hover:shadow-md transition-shadow">
          <div className="text-blue-600 mb-4">
            <Upload className="h-8 w-8" />
          </div>
          <h3 className="text-xl font-semibold text-gray-900 mb-2">Upload Documents</h3>
          <p className="text-gray-600">
            Drag and drop or select your PDF files to begin building your personalized knowledge base
          </p>
        </div>

        <div className="bg-white rounded-xl p-8 shadow-sm border border-gray-200 hover:shadow-md transition-shadow">
          <div className="text-emerald-600 mb-4">
            <BookOpen className="h-8 w-8" />
          </div>
          <h3 className="text-xl font-semibold text-gray-900 mb-2">Ask Questions</h3>
          <p className="text-gray-600">
            Get instant, contextual answers from your materials with precise source citations
          </p>
        </div>
      </div>

      <div className={`bg-white rounded-2xl shadow-lg border-2 border-dashed transition-all duration-300 ${
        isDragging ? 'border-blue-400 bg-blue-50 scale-105' : 'border-gray-300 hover:border-blue-400 hover:shadow-xl'
      }`}>
        <div
          className="p-12 text-center"
          onDragOver={handleDragOver}
          onDragLeave={handleDragLeave}
          onDrop={handleDrop}
        >
          {uploadProgress !== null ? (
            <div className="space-y-6">
              {uploadComplete ? (
                <div className="text-emerald-600">
                  <Check className="h-16 w-16 mx-auto mb-4" />
                  <h3 className="text-2xl font-semibold">Upload Complete!</h3>
                  <p className="text-gray-600">Your documents have been processed and are ready for use</p>
                </div>
              ) : (
                <div className="text-blue-600">
                  <Upload className="h-16 w-16 mx-auto mb-4 animate-pulse" />
                  <h3 className="text-2xl font-semibold mb-2">Processing Documents...</h3>
                  <div className="w-full bg-gray-200 rounded-full h-3 max-w-md mx-auto">
                    <div 
                      className="bg-blue-600 h-3 rounded-full transition-all duration-300"
                      style={{ width: `${uploadProgress}%` }}
                    ></div>
                  </div>
                  <p className="text-gray-600 mt-2">{uploadProgress}% complete</p>
                </div>
              )}
            </div>
          ) : (
            <div className="space-y-6">
              <div className="text-gray-400">
                <FileText className="h-16 w-16 mx-auto mb-4" />
              </div>
              <div>
                <h3 className="text-2xl font-semibold text-gray-900 mb-2">
                  Drop your PDF files here
                </h3>
                <p className="text-gray-600 mb-6">
                  or click to browse and select your study materials
                </p>
                <label className="inline-flex items-center px-6 py-3 bg-blue-600 text-white rounded-lg font-medium hover:bg-blue-700 transition-colors cursor-pointer shadow-lg hover:shadow-xl">
                  <Upload className="h-5 w-5 mr-2" />
                  Choose Files
                  <input
                    type="file"
                    accept=".pdf"
                    multiple
                    onChange={handleFileInput}
                    className="hidden"
                  />
                </label>
              </div>
              <div className="text-sm text-gray-500">
                Supports PDF files up to 50MB each
              </div>
            </div>
          )}
        </div>
      </div>

      {documents.length > 0 && (
        <div className="mt-8 text-center">
          <button
            onClick={onViewLibrary}
            className="inline-flex items-center px-6 py-3 bg-gray-900 text-white rounded-lg font-medium hover:bg-gray-800 transition-colors shadow-lg hover:shadow-xl"
          >
            View Document Library
            <ArrowRight className="h-5 w-5 ml-2" />
          </button>
        </div>
      )}
    </div>
  );
};
```

### src/components/DocumentLibrary.tsx
```typescript
import React from 'react';
import { FileText, Calendar, Hash, Users, ArrowRight, Plus, CheckCircle } from 'lucide-react';
import { Document } from '../types';

interface DocumentLibraryProps {
  documents: Document[];
  selectedDocuments: Document[];
  onDocumentSelect: (document: Document) => void;
  onStartChat: () => void;
  onUploadMore: () => void;
}

export const DocumentLibrary: React.FC<DocumentLibraryProps> = ({
  documents,
  selectedDocuments,
  onDocumentSelect,
  onStartChat,
  onUploadMore
}) => {
  const isSelected = (document: Document) => 
    selectedDocuments.some(d => d.id === document.id);

  return (
    <div className="max-w-6xl mx-auto">
      <div className="mb-8">
        <div className="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
          <div>
            <h2 className="text-3xl font-bold text-gray-900 mb-2">Document Library</h2>
            <p className="text-gray-600">
              Select documents to include in your study session, then start asking questions
            </p>
          </div>
          <button
            onClick={onUploadMore}
            className="inline-flex items-center px-4 py-2 bg-gray-100 hover:bg-gray-200 text-gray-700 rounded-lg font-medium transition-colors"
          >
            <Plus className="h-5 w-5 mr-2" />
            Upload More
          </button>
        </div>
      </div>

      {selectedDocuments.length > 0 && (
        <div className="mb-8 p-6 bg-blue-50 border border-blue-200 rounded-xl">
          <div className="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
            <div className="flex items-center space-x-3">
              <div className="flex -space-x-2">
                {selectedDocuments.slice(0, 3).map((doc, index) => (
                  <div
                    key={doc.id}
                    className="w-8 h-8 bg-blue-600 text-white rounded-full flex items-center justify-center text-sm font-medium border-2 border-white"
                  >
                    {index + 1}
                  </div>
                ))}
                {selectedDocuments.length > 3 && (
                  <div className="w-8 h-8 bg-gray-400 text-white rounded-full flex items-center justify-center text-xs font-medium border-2 border-white">
                    +{selectedDocuments.length - 3}
                  </div>
                )}
              </div>
              <div>
                <p className="font-semibold text-blue-900">
                  {selectedDocuments.length} document{selectedDocuments.length !== 1 ? 's' : ''} selected
                </p>
                <p className="text-blue-700 text-sm">Ready to start your study session</p>
              </div>
            </div>
            <button
              onClick={onStartChat}
              className="inline-flex items-center px-6 py-3 bg-blue-600 hover:bg-blue-700 text-white rounded-lg font-medium transition-colors"
            >
              Start Chat Session
              <ArrowRight className="h-5 w-5 ml-2" />
            </button>
          </div>
        </div>
      )}

      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {documents.map((document) => (
          <div
            key={document.id}
            onClick={() => onDocumentSelect(document)}
            className={`relative bg-white rounded-xl border-2 p-6 cursor-pointer transition-all duration-200 hover:shadow-lg group ${
              isSelected(document)
                ? 'border-blue-500 bg-blue-50 shadow-md'
                : 'border-gray-200 hover:border-gray-300'
            }`}
          >
            {isSelected(document) && (
              <div className="absolute top-4 right-4">
                <CheckCircle className="h-6 w-6 text-blue-600" />
              </div>
            )}
            
            <div className="flex items-start space-x-4">
              <div className={`p-3 rounded-lg ${
                isSelected(document) ? 'bg-blue-100' : 'bg-gray-100 group-hover:bg-blue-100'
              } transition-colors`}>
                <FileText className={`h-6 w-6 ${
                  isSelected(document) ? 'text-blue-600' : 'text-gray-600 group-hover:text-blue-600'
                } transition-colors`} />
              </div>
              
              <div className="flex-1 min-w-0">
                <h3 className={`font-semibold text-lg mb-2 line-clamp-2 ${
                  isSelected(document) ? 'text-blue-900' : 'text-gray-900'
                }`}>
                  {document.name}
                </h3>
                
                <div className="space-y-2">
                  <div className="flex items-center space-x-4 text-sm text-gray-600">
                    <div className="flex items-center space-x-1">
                      <Hash className="h-4 w-4" />
                      <span>{document.pageCount} pages</span>
                    </div>
                    <div className="flex items-center space-x-1">
                      <Users className="h-4 w-4" />
                      <span>{document.size}</span>
                    </div>
                  </div>
                  
                  <div className="flex items-center space-x-1 text-sm text-gray-500">
                    <Calendar className="h-4 w-4" />
                    <span>Uploaded {new Date(document.uploadDate).toLocaleDateString()}</span>
                  </div>
                </div>
              </div>
            </div>

            <div className={`mt-4 pt-4 border-t transition-colors ${
              isSelected(document) ? 'border-blue-200' : 'border-gray-200'
            }`}>
              <div className={`text-sm font-medium ${
                isSelected(document) ? 'text-blue-700' : 'text-gray-700'
              }`}>
                {isSelected(document) ? 'Selected for chat session' : 'Click to select'}
              </div>
            </div>
          </div>
        ))}
      </div>

      {documents.length === 0 && (
        <div className="text-center py-12">
          <FileText className="h-16 w-16 text-gray-300 mx-auto mb-4" />
          <h3 className="text-xl font-semibold text-gray-900 mb-2">No documents yet</h3>
          <p className="text-gray-600 mb-6">Upload your first PDF to get started with StudyMate</p>
          <button
            onClick={onUploadMore}
            className="inline-flex items-center px-6 py-3 bg-blue-600 hover:bg-blue-700 text-white rounded-lg font-medium transition-colors"
          >
            <Upload className="h-5 w-5 mr-2" />
            Upload Documents
          </button>
        </div>
      )}
    </div>
  );
};
```

### src/components/ChatInterface.tsx
```typescript
import React, { useState, useRef, useEffect } from 'react';
import { Send, ArrowLeft, FileText, ExternalLink, BookOpen, MessageCircle, Loader2 } from 'lucide-react';
import { Document, ChatMessage } from '../types';

interface ChatInterfaceProps {
  selectedDocuments: Document[];
  messages: ChatMessage[];
  isTyping: boolean;
  onSendMessage: (message: string) => void;
  onBackToLibrary: () => void;
}

export const ChatInterface: React.FC<ChatInterfaceProps> = ({
  selectedDocuments,
  messages,
  isTyping,
  onSendMessage,
  onBackToLibrary
}) => {
  const [inputMessage, setInputMessage] = useState('');
  const messagesEndRef = useRef<HTMLDivElement>(null);

  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages, isTyping]);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (inputMessage.trim() && !isTyping) {
      onSendMessage(inputMessage.trim());
      setInputMessage('');
    }
  };

  const suggestedQuestions = [
    "What are the main concepts covered in these documents?",
    "Can you summarize the key points from chapter 1?",
    "What is the relationship between these topics?",
    "Help me understand this specific concept better"
  ];

  return (
    <div className="max-w-5xl mx-auto h-[calc(100vh-200px)] flex flex-col">
      {/* Header */}
      <div className="bg-white rounded-t-xl border border-gray-200 p-6">
        <div className="flex items-center justify-between mb-4">
          <button
            onClick={onBackToLibrary}
            className="inline-flex items-center text-gray-600 hover:text-gray-900 transition-colors"
          >
            <ArrowLeft className="h-5 w-5 mr-2" />
            Back to Library
          </button>
          <div className="flex items-center space-x-2 text-emerald-600">
            <MessageCircle className="h-5 w-5" />
            <span className="font-medium">Chat Active</span>
          </div>
        </div>

        <div className="flex items-center space-x-4">
          <div className="flex items-center space-x-2">
            <BookOpen className="h-6 w-6 text-blue-600" />
            <span className="font-semibold text-gray-900">Study Session</span>
          </div>
          <div className="text-sm text-gray-600">
            {selectedDocuments.length} document{selectedDocuments.length !== 1 ? 's' : ''} loaded
          </div>
        </div>

        <div className="mt-4 flex flex-wrap gap-2">
          {selectedDocuments.map((doc) => (
            <div
              key={doc.id}
              className="inline-flex items-center px-3 py-1 bg-blue-100 text-blue-800 rounded-full text-sm"
            >
              <FileText className="h-4 w-4 mr-1" />
              <span className="truncate max-w-32">{doc.name.replace('.pdf', '')}</span>
            </div>
          ))}
        </div>
      </div>

      {/* Messages */}
      <div className="flex-1 bg-gray-50 border-x border-gray-200 overflow-y-auto">
        <div className="p-6 space-y-6">
          {messages.length === 0 ? (
            <div className="text-center py-12">
              <BookOpen className="h-16 w-16 text-gray-300 mx-auto mb-4" />
              <h3 className="text-xl font-semibold text-gray-900 mb-2">Ready to Help</h3>
              <p className="text-gray-600 mb-8 max-w-md mx-auto">
                Ask me anything about your uploaded documents. I'll provide detailed answers with source references.
              </p>
              
              <div className="grid grid-cols-1 sm:grid-cols-2 gap-3 max-w-2xl mx-auto">
                {suggestedQuestions.map((question, index) => (
                  <button
                    key={index}
                    onClick={() => setInputMessage(question)}
                    className="p-3 text-left bg-white border border-gray-200 rounded-lg hover:border-blue-300 hover:shadow-sm transition-all text-sm text-gray-700"
                  >
                    {question}
                  </button>
                ))}
              </div>
            </div>
          ) : (
            messages.map((message) => (
              <div
                key={message.id}
                className={`flex ${message.type === 'user' ? 'justify-end' : 'justify-start'}`}
              >
                <div className={`max-w-3xl ${message.type === 'user' ? 'order-2' : 'order-1'}`}>
                  <div
                    className={`px-6 py-4 rounded-2xl ${
                      message.type === 'user'
                        ? 'bg-blue-600 text-white'
                        : 'bg-white border border-gray-200 text-gray-900'
                    }`}
                  >
                    <p className="leading-relaxed">{message.content}</p>
                  </div>

                  {message.sources && message.sources.length > 0 && (
                    <div className="mt-3 space-y-2">
                      <p className="text-sm font-medium text-gray-700">Sources:</p>
                      {message.sources.map((source, index) => (
                        <div
                          key={index}
                          className="bg-gray-100 border border-gray-200 rounded-lg p-3 text-sm"
                        >
                          <div className="flex items-center justify-between mb-2">
                            <div className="flex items-center space-x-2">
                              <FileText className="h-4 w-4 text-blue-600" />
                              <span className="font-medium text-gray-900 truncate">
                                {source.documentName}
                              </span>
                            </div>
                            <div className="flex items-center space-x-1 text-blue-600">
                              <span>Page {source.pageNumber}</span>
                              <ExternalLink className="h-3 w-3" />
                            </div>
                          </div>
                          <p className="text-gray-700 italic">"{source.snippet}"</p>
                        </div>
                      ))}
                    </div>
                  )}

                  <div className="mt-2 text-xs text-gray-500">
                    {new Date(message.timestamp).toLocaleTimeString()}
                  </div>
                </div>
              </div>
            ))
          )}

          {isTyping && (
            <div className="flex justify-start">
              <div className="max-w-3xl">
                <div className="bg-white border border-gray-200 rounded-2xl px-6 py-4">
                  <div className="flex items-center space-x-2 text-gray-600">
                    <Loader2 className="h-5 w-5 animate-spin" />
                    <span>StudyMate is thinking...</span>
                  </div>
                </div>
              </div>
            </div>
          )}
        </div>
        <div ref={messagesEndRef} />
      </div>

      {/* Input */}
      <form onSubmit={handleSubmit} className="bg-white rounded-b-xl border border-gray-200 p-6">
        <div className="flex space-x-4">
          <div className="flex-1 relative">
            <input
              type="text"
              value={inputMessage}
              onChange={(e) => setInputMessage(e.target.value)}
              placeholder="Ask a question about your documents..."
              className="w-full px-4 py-3 pr-12 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              disabled={isTyping}
            />
            <button
              type="submit"
              disabled={!inputMessage.trim() || isTyping}
              className="absolute right-2 top-1/2 transform -translate-y-1/2 p-2 text-blue-600 hover:text-blue-700 disabled:text-gray-400 disabled:cursor-not-allowed transition-colors"
            >
              <Send className="h-5 w-5" />
            </button>
          </div>
        </div>
        
        <div className="mt-3 flex flex-wrap gap-2">
          {messages.length === 0 && suggestedQuestions.slice(0, 2).map((question, index) => (
            <button
              key={index}
              type="button"
              onClick={() => setInputMessage(question)}
              className="px-3 py-1 text-xs bg-gray-100 hover:bg-gray-200 text-gray-700 rounded-full transition-colors"
            >
              {question}
            </button>
          ))}
        </div>
      </form>
    </div>
  );
};
```

### src/utils/mockData.ts
```typescript
import { Document, ChatMessage } from '../types';

export const mockDocuments: Document[] = [
  {
    id: '1',
    name: 'Introduction to Machine Learning.pdf',
    size: '2.4 MB',
    uploadDate: '2025-01-08',
    pageCount: 156,
    type: 'pdf'
  },
  {
    id: '2',
    name: 'Linear Algebra Fundamentals.pdf',
    size: '1.8 MB',
    uploadDate: '2025-01-07',
    pageCount: 89,
    type: 'pdf'
  },
  {
    id: '3',
    name: 'Statistics and Probability.pdf',
    size: '3.1 MB',
    uploadDate: '2025-01-06',
    pageCount: 201,
    type: 'pdf'
  },
  {
    id: '4',
    name: 'Calculus for Engineering.pdf',
    size: '4.2 MB',
    uploadDate: '2025-01-05',
    pageCount: 278,
    type: 'pdf'
  }
];

export const mockResponses = [
  {
    keywords: ["supervised", "learning", "machine", "algorithm", "training"],
    answer: "Supervised learning is a machine learning paradigm where algorithms learn from labeled training data to make predictions or decisions on new, unseen data. The algorithm learns to map inputs to outputs based on example input-output pairs provided during training. Common examples include classification (predicting categories) and regression (predicting continuous values).",
    sources: [
      {
        documentId: '1',
        documentName: 'Introduction to Machine Learning.pdf',
        pageNumber: 23,
        snippet: "Supervised learning algorithms use labeled examples to learn a mapping function from inputs to outputs, enabling accurate predictions on new data."
      }
    ]
  },
  {
    keywords: ["matrix", "matrices", "linear", "algebra", "vector"],
    answer: "A matrix is a rectangular array of numbers, symbols, or expressions arranged in rows and columns. Matrices are fundamental mathematical objects used extensively in linear algebra for representing linear transformations, solving systems of equations, and organizing data. Matrix operations like addition, multiplication, and inversion are essential tools in many areas of mathematics and computer science.",
    sources: [
      {
        documentId: '2',
        documentName: 'Linear Algebra Fundamentals.pdf',
        pageNumber: 12,
        snippet: "A matrix is defined as a rectangular arrangement of elements organized in rows and columns, forming the foundation for linear transformations and vector spaces."
      }
    ]
  },
  {
    keywords: ["probability", "statistics", "distribution", "random", "variable"],
    answer: "Probability is the mathematical study of randomness and uncertainty. It provides a framework for quantifying how likely events are to occur. Key concepts include probability distributions, which describe how probabilities are distributed over possible outcomes, and random variables, which assign numerical values to outcomes of random phenomena.",
    sources: [
      {
        documentId: '3',
        documentName: 'Statistics and Probability.pdf',
        pageNumber: 34,
        snippet: "Probability theory provides the mathematical foundation for dealing with uncertainty and randomness in statistical analysis."
      }
    ]
  },
  {
    keywords: ["derivative", "calculus", "differentiation", "rate", "change"],
    answer: "A derivative represents the rate of change of a function with respect to its input variable. It's a fundamental concept in calculus that measures how a function's output changes as its input changes. Geometrically, the derivative at a point gives the slope of the tangent line to the function's graph at that point. Derivatives are essential for optimization, physics, and engineering applications.",
    sources: [
      {
        documentId: '4',
        documentName: 'Calculus for Engineering.pdf',
        pageNumber: 67,
        snippet: "The derivative of a function f(x) at point x=a represents the instantaneous rate of change and is defined as the limit of the difference quotient."
      }
    ]
  }
];

export const generateMockResponse = (question: string): ChatMessage => {
  const questionLower = question.toLowerCase();
  
  // Find the most relevant response based on keywords
  let bestMatch = mockResponses[0];
  let maxScore = 0;
  
  for (const response of mockResponses) {
    const score = response.keywords.reduce((acc, keyword) => {
      return acc + (questionLower.includes(keyword.toLowerCase()) ? 1 : 0);
    }, 0);
    
    if (score > maxScore) {
      maxScore = score;
      bestMatch = response;
    }
  }
  
  // If no keywords match, use a generic response
  if (maxScore === 0) {
    bestMatch = {
      keywords: [],
      answer: "That's an interesting question! Based on the documents you've uploaded, I can help provide context and explanations. Could you be more specific about which concept or topic you'd like me to focus on? I can reference specific sections from your materials to give you a comprehensive answer.",
      sources: [
        {
          documentId: mockDocuments[0]?.id || '1',
          documentName: mockDocuments[0]?.name || 'Study Material.pdf',
          pageNumber: Math.floor(Math.random() * 100) + 1,
          snippet: "This section contains relevant information that addresses your question about the academic concepts covered in this material."
        }
      ]
    };
  }
  
  return {
    id: Date.now().toString(),
    type: 'assistant',
    content: bestMatch.answer,
    timestamp: new Date().toISOString(),
    sources: bestMatch.sources
  };
};
```

## Installation Instructions

1. Clone or download the project
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```
4. Build for production:
   ```bash
   npm run build
   ```

## Features

- **Document Upload**: Drag & drop or file selection for PDF uploads
- **Document Library**: Visual library with multi-select functionality
- **AI Chat Interface**: Conversational Q&A with source citations
- **Responsive Design**: Works on all device sizes
- **Mock AI Responses**: Intelligent keyword-based response system
- **Source Citations**: Every AI response includes page references
- **Real-time Typing Indicators**: Shows when AI is processing
- **Smooth Animations**: Professional transitions and micro-interactions

## Technology Stack

- **Frontend**: React 18 + TypeScript
- **Styling**: Tailwind CSS
- **Icons**: Lucide React
- **Build Tool**: Vite
- **Linting**: ESLint with TypeScript support

## Live Demo

The application is deployed and available at: https://studymate-ai-academi-407a.bolt.host