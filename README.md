<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ProCV - Interactive Resume & CV Builder</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Font Awesome CDN for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=EB+Garamond:wght@400;600;700&family=Georgia&family=Inter:wght@300;400;500;600;700&family=Lato:wght@300;400;700&family=Montserrat:wght@400;500;600;700&family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    
    <!-- LZ-String Library for URL Compression -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lz-string/1.5.0/lz-string.min.js"></script>

    <!-- Vue 3 Global Build CDN for reactivity -->
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

    <style>
        /* Base page styles */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f1f5f9;
            color: #0f172a;
        }

        /* Custom scrollbar for editor panel */
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }

        /* Paper sheet preview dimensions */
        .cv-paper {
            width: 210mm;
            min-height: 297mm;
            background: #ffffff;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            transition: all 0.2s ease;
        }

        /* PRINT STYLING - Standard A4 PDF Output */
        @media print {
            @page {
                size: A4 portrait;
                margin: 0;
            }
            body {
                background: #ffffff !important;
                padding: 0 !important;
                margin: 0 !important;
                overflow: visible !important;
            }
            .no-print {
                display: none !important;
            }
            .print-container {
                padding: 0 !important;
                margin: 0 !important;
                width: 100% !important;
                max-width: 100% !important;
                overflow: visible !important;
            }
            .cv-paper {
                box-shadow: none !important;
                width: 100% !important;
                min-height: 100vh !important;
                margin: 0 !important;
                padding: 0 !important;
                border: none !important;
                border-radius: 0 !important;
            }
            * {
                -webkit-print-color-adjust: exact !important;
                print-color-adjust: exact !important;
            }
        }
    </style>
</head>
<body class="h-screen flex flex-col overflow-hidden">
    <div id="app" class="h-full flex flex-col">
        <!-- Top Header / Controls Bar -->
        <header class="no-print bg-slate-900 text-white border-b border-slate-800 px-4 py-3 flex-shrink-0 z-30">
            <div class="max-w-7xl mx-auto flex flex-wrap items-center justify-between gap-3">
                <!-- App Logo -->
                <div class="flex items-center space-x-3">
                    <div class="w-9 h-9 rounded-lg bg-indigo-600 flex items-center justify-center font-bold text-lg shadow-md">
                        <i class="fa-solid fa-file-invoice text-white"></i>
                    </div>
                    <div>
                        <h1 class="text-lg font-bold leading-none tracking-tight">ProCV Builder</h1>
                        <p class="text-xs text-slate-400">Live Resume & Portfolio Generator</p>
                    </div>
                </div>

                <!-- View Mode Switcher for Mobile / Desktop -->
                <div class="flex bg-slate-800 p-1 rounded-lg border border-slate-700">
                    <button @click="viewMode = 'edit'" :class="{'bg-indigo-600 text-white shadow': viewMode === 'edit', 'text-slate-400 hover:text-white': viewMode !== 'edit'}" class="px-3 py-1.5 text-xs font-medium rounded-md transition flex items-center space-x-1.5">
                        <i class="fa-solid fa-pen-to-square"></i>
                        <span class="hidden sm:inline">Editor</span>
                    </button>
                    <button @click="viewMode = 'split'" :class="{'bg-indigo-600 text-white shadow': viewMode === 'split', 'text-slate-400 hover:text-white': viewMode !== 'split'}" class="hidden lg:flex px-3 py-1.5 text-xs font-medium rounded-md transition items-center space-x-1.5">
                        <i class="fa-solid fa-columns"></i>
                        <span>Split View</span>
                    </button>
                    <button @click="viewMode = 'preview'" :class="{'bg-indigo-600 text-white shadow': viewMode === 'preview', 'text-slate-400 hover:text-white': viewMode !== 'preview'}" class="px-3 py-1.5 text-xs font-medium rounded-md transition flex items-center space-x-1.5">
                        <i class="fa-solid fa-eye"></i>
                        <span class="hidden sm:inline">Preview</span>
                    </button>
                </div>

                <!-- Action Buttons -->
                <div class="flex items-center space-x-2">
                    <!-- Share / Link Generator Button -->
                    <button @click="generateShareLink" class="bg-indigo-600 hover:bg-indigo-500 text-white px-3 py-1.5 rounded-lg text-xs font-semibold shadow-sm transition flex items-center space-x-1.5" title="Generate shareable URL link">
                        <i class="fa-solid fa-share-nodes"></i>
                        <span>Share Link</span>
                    </button>

                    <button @click="loadSampleData" class="bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 px-3 py-1.5 rounded-lg text-xs font-medium transition flex items-center space-x-1.5" title="Pre-fill standard demo CV data">
                        <i class="fa-solid fa-wand-magic-sparkles text-amber-400"></i>
                        <span class="hidden md:inline">Sample Data</span>
                    </button>

                    <button @click="exportJSON" class="bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 px-3 py-1.5 rounded-lg text-xs font-medium transition flex items-center space-x-1.5" title="Export json configuration file">
                        <i class="fa-solid fa-download"></i>
                        <span class="hidden md:inline">Export</span>
                    </button>

                    <label class="bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 px-3 py-1.5 rounded-lg text-xs font-medium transition cursor-pointer flex items-center space-x-1.5" title="Import json file">
                        <i class="fa-solid fa-upload"></i>
                        <span class="hidden md:inline">Import</span>
                        <input type="file" accept=".json" @change="importJSON" class="hidden">
                    </label>

                    <button @click="clearForm" class="bg-rose-950/40 hover:bg-rose-900/60 text-rose-300 border border-rose-800/60 px-3 py-1.5 rounded-lg text-xs font-medium transition flex items-center space-x-1.5" title="Clear form">
                        <i class="fa-solid fa-trash-can"></i>
                    </button>

                    <button @click="printCV" class="bg-emerald-600 hover:bg-emerald-500 text-white px-4 py-1.5 rounded-lg text-xs font-semibold shadow-sm transition flex items-center space-x-2">
                        <i class="fa-solid fa-print"></i>
                        <span>Download PDF / Print</span>
                    </button>
                </div>
            </div>
        </header>

        <!-- Notification Banner when viewing shared link -->
        <div v-if="isSharedView" class="no-print bg-indigo-900 text-indigo-100 px-4 py-2 text-xs flex items-center justify-between border-b border-indigo-800">
            <div class="flex items-center space-x-2">
                <i class="fa-solid fa-link text-indigo-300"></i>
                <span>You are currently viewing a <strong>shared CV link</strong>. Any edits will be saved locally.</span>
            </div>
            <button @click="clearUrlHash" class="text-indigo-200 hover:text-white underline font-semibold ml-4">
                Clear Link State
            </button>
        </div>

        <!-- Notification Toast Popup -->
        <div v-if="toastMessage" class="fixed bottom-5 right-5 z-50 bg-slate-900 text-white border border-slate-700 px-4 py-3 rounded-xl shadow-2xl flex items-center space-x-3 transition-all transform duration-300">
            <i class="fa-solid fa-circle-check text-emerald-400 text-lg"></i>
            <span class="text-xs font-medium">{{ toastMessage }}</span>
        </div>

        <!-- Shareable Link Modal Overlay -->
        <div v-if="showShareModal" class="fixed inset-0 z-50 bg-slate-950/70 backdrop-blur-sm flex items-center justify-center p-4">
            <div class="bg-white rounded-2xl max-w-lg w-full p-6 shadow-2xl border border-slate-100 space-y-4 relative">
                <button @click="showShareModal = false" class="absolute top-4 right-4 text-slate-400 hover:text-slate-600">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>

                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 rounded-full bg-indigo-100 text-indigo-600 flex items-center justify-center text-lg">
                        <i class="fa-solid fa-share-nodes"></i>
                    </div>
                    <div>
                        <h3 class="text-base font-bold text-slate-800">Share Your CV</h3>
                        <p class="text-xs text-slate-500">Anyone with this link can view and edit a copy of your resume.</p>
                    </div>
                </div>

                <div class="space-y-2">
                    <label class="block text-xs font-semibold text-slate-700">Compressed CV URL</label>
                    <div class="flex items-center space-x-2">
                        <input type="text" readonly :value="shareableUrl" class="w-full text-xs font-mono bg-slate-100 border border-slate-300 rounded-lg p-2.5 text-slate-700 focus:outline-none">
                        <button @click="copyShareUrl" class="bg-indigo-600 hover:bg-indigo-500 text-white px-4 py-2.5 rounded-lg text-xs font-semibold whitespace-nowrap transition flex items-center space-x-1.5">
                            <i class="fa-solid fa-copy"></i>
                            <span>{{ copyButtonText }}</span>
                        </button>
                    </div>
                </div>

                <p class="text-[11px] text-slate-400 leading-normal">
                    <i class="fa-solid fa-circle-info text-indigo-500 mr-1"></i>
                    This URL contains all your resume data compressed directly inside the link. No database or cloud registration required!
                </p>
            </div>
        </div>
            </div>
        </header>

        <!-- Main Workspace Area -->
        <main class="flex-1 flex overflow-hidden relative">
            
            <!-- LEFT PANE: Form Controls & Editors -->
            <section v-show="viewMode === 'edit' || viewMode === 'split'" 
                     :class="{'w-full lg:w-1/2': viewMode === 'split', 'w-full': viewMode === 'edit'}" 
                     class="no-print h-full overflow-y-auto custom-scrollbar border-r border-slate-200 bg-slate-50 p-4 sm:p-6 transition-all duration-300">
                
                <div class="max-w-2xl mx-auto space-y-6 pb-20">
                    
                    <!-- Design & Theme Settings Accordion -->
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                        <div @click="toggleAccordion('settings')" class="p-4 bg-slate-100/70 border-b border-slate-200 flex items-center justify-between cursor-pointer hover:bg-slate-100">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-md bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-xs">
                                    <i class="fa-solid fa-palette"></i>
                                </span>
                                <h2 class="font-bold text-slate-800 text-sm">Template, Theme & Typography</h2>
                            </div>
                            <i :class="activeAccordion === 'settings' ? 'fa-chevron-up' : 'fa-chevron-down'" class="fa-solid text-slate-400 text-xs"></i>
                        </div>

                        <div v-show="activeAccordion === 'settings'" class="p-4 space-y-5">
                            <!-- Template Selection -->
                            <div>
                                <label class="block text-xs font-semibold text-slate-700 mb-2">Layout Template</label>
                                <div class="grid grid-cols-2 sm:grid-cols-4 gap-2">
                                    <button v-for="t in templates" :key="t.id" @click="cvData.settings.template = t.id"
                                            :class="cvData.settings.template === t.id ? 'border-indigo-600 bg-indigo-50/50 text-indigo-700 font-bold' : 'border-slate-200 hover:border-slate-300 text-slate-600'"
                                            class="p-2 border rounded-lg text-xs text-center transition">
                                        {{ t.name }}
                                    </button>
                                </div>
                            </div>

                            <!-- Color Palette Selector -->
                            <div>
                                <label class="block text-xs font-semibold text-slate-700 mb-2">Accent Color</label>
                                <div class="flex flex-wrap gap-3">
                                    <button v-for="color in themeColors" :key="color.hex" @click="cvData.settings.themeColor = color.hex"
                                            :style="{ backgroundColor: color.hex }"
                                            :class="cvData.settings.themeColor === color.hex ? 'ring-2 ring-offset-2 ring-indigo-600 scale-110' : 'hover:scale-105'"
                                            class="w-7 h-7 rounded-full transition shadow-sm relative flex items-center justify-center"
                                            :title="color.name">
                                        <i v-if="cvData.settings.themeColor === color.hex" class="fa-solid fa-check text-white text-xs"></i>
                                    </button>
                                </div>
                            </div>

                            <!-- Font Family Selection -->
                            <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">Font Family</label>
                                    <select v-model="cvData.settings.font" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                        <option value="Inter">Inter (Sans-Serif)</option>
                                        <option value="Roboto">Roboto (Clean)</option>
                                        <option value="Montserrat">Montserrat (Modern)</option>
                                        <option value="Lato">Lato (Friendly)</option>
                                        <option value="Georgia">Georgia (Serif)</option>
                                        <option value="EB Garamond">Garamond (Classic)</option>
                                    </select>
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">Font Scale</label>
                                    <select v-model="cvData.settings.fontSize" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                        <option value="small">Compact (Small)</option>
                                        <option value="normal">Standard (Medium)</option>
                                        <option value="large">Spacious (Large)</option>
                                    </select>
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">Section Spacing</label>
                                    <select v-model="cvData.settings.spacing" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                        <option value="tight">Tight</option>
                                        <option value="normal">Normal</option>
                                        <option value="relaxed">Relaxed</option>
                                    </select>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Personal Information Accordion -->
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                        <div @click="toggleAccordion('personal')" class="p-4 bg-slate-100/70 border-b border-slate-200 flex items-center justify-between cursor-pointer hover:bg-slate-100">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-md bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-xs">
                                    <i class="fa-solid fa-user"></i>
                                </span>
                                <h2 class="font-bold text-slate-800 text-sm">Header & Personal Information</h2>
                            </div>
                            <i :class="activeAccordion === 'personal' ? 'fa-chevron-up' : 'fa-chevron-down'" class="fa-solid text-slate-400 text-xs"></i>
                        </div>

                        <div v-show="activeAccordion === 'personal'" class="p-4 space-y-4">
                            <!-- Profile Photo Upload -->
                            <div class="flex items-center space-x-4 pb-2 border-b border-slate-100">
                                <div class="w-16 h-16 rounded-full bg-slate-100 border border-slate-300 flex items-center justify-center overflow-hidden flex-shrink-0 relative group">
                                    <img v-if="cvData.personal.photo" :src="cvData.personal.photo" alt="Profile Photo" class="w-full h-full object-cover">
                                    <i v-else class="fa-solid fa-user text-slate-300 text-2xl"></i>
                                </div>
                                <div class="space-y-1">
                                    <label class="block text-xs font-semibold text-slate-700">Profile Photo</label>
                                    <div class="flex items-center space-x-2">
                                        <label class="cursor-pointer bg-slate-100 hover:bg-slate-200 text-slate-700 px-3 py-1 rounded text-xs border border-slate-300 transition">
                                            <span>Upload Image</span>
                                            <input type="file" accept="image/*" @change="uploadPhoto" class="hidden">
                                        </label>
                                        <button v-if="cvData.personal.photo" @click="cvData.personal.photo = ''" class="text-xs text-rose-600 hover:underline">Remove</button>
                                    </div>
                                    <p class="text-[10px] text-slate-400">JPG or PNG (Square ratio recommended)</p>
                                </div>
                            </div>

                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">Full Name</label>
                                    <input type="text" v-model="cvData.personal.fullName" placeholder="e.g. John Doe" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">Professional Job Title</label>
                                    <input type="text" v-model="cvData.personal.jobTitle" placeholder="e.g. Senior Software Engineer" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">Email Address</label>
                                    <input type="email" v-model="cvData.personal.email" placeholder="john@example.com" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">Phone Number</label>
                                    <input type="text" v-model="cvData.personal.phone" placeholder="+1 (555) 000-0000" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">Location / Address</label>
                                    <input type="text" v-model="cvData.personal.location" placeholder="San Francisco, CA" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">Website / Portfolio</label>
                                    <input type="text" v-model="cvData.personal.website" placeholder="johndoe.com" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">LinkedIn Profile</label>
                                    <input type="text" v-model="cvData.personal.linkedin" placeholder="linkedin.com/in/johndoe" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                </div>

                                <div>
                                    <label class="block text-xs font-semibold text-slate-700 mb-1">GitHub / Social Link</label>
                                    <input type="text" v-model="cvData.personal.github" placeholder="github.com/johndoe" class="w-full text-xs border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Professional Summary Accordion -->
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                        <div @click="toggleAccordion('summary')" class="p-4 bg-slate-100/70 border-b border-slate-200 flex items-center justify-between cursor-pointer hover:bg-slate-100">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-md bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-xs">
                                    <i class="fa-solid fa-align-left"></i>
                                </span>
                                <h2 class="font-bold text-slate-800 text-sm">Professional Summary</h2>
                            </div>
                            <i :class="activeAccordion === 'summary' ? 'fa-chevron-up' : 'fa-chevron-down'" class="fa-solid text-slate-400 text-xs"></i>
                        </div>

                        <div v-show="activeAccordion === 'summary'" class="p-4">
                            <textarea v-model="cvData.summary" rows="4" placeholder="Write a brief, high-impact summary of your experience, key skills, and professional highlights..." class="w-full text-xs border border-slate-300 rounded-lg p-2.5 focus:ring-2 focus:ring-indigo-500 focus:outline-none leading-relaxed"></textarea>
                        </div>
                    </div>

                    <!-- Work Experience Accordion -->
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                        <div @click="toggleAccordion('experience')" class="p-4 bg-slate-100/70 border-b border-slate-200 flex items-center justify-between cursor-pointer hover:bg-slate-100">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-md bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-xs">
                                    <i class="fa-solid fa-briefcase"></i>
                                </span>
                                <h2 class="font-bold text-slate-800 text-sm">Work Experience ({{ cvData.experience.length }})</h2>
                            </div>
                            <i :class="activeAccordion === 'experience' ? 'fa-chevron-up' : 'fa-chevron-down'" class="fa-solid text-slate-400 text-xs"></i>
                        </div>

                        <div v-show="activeAccordion === 'experience'" class="p-4 space-y-4">
                            <div v-for="(exp, index) in cvData.experience" :key="index" class="p-3 bg-slate-50 border border-slate-200 rounded-lg space-y-3 relative">
                                <div class="flex justify-between items-center border-b border-slate-200 pb-2">
                                    <span class="text-xs font-bold text-slate-700">Position #{{ index + 1 }}</span>
                                    <div class="flex items-center space-x-1">
                                        <button @click="moveItem(cvData.experience, index, -1)" :disabled="index === 0" class="p-1 text-slate-400 hover:text-indigo-600 disabled:opacity-30 text-xs">
                                            <i class="fa-solid fa-arrow-up"></i>
                                        </button>
                                        <button @click="moveItem(cvData.experience, index, 1)" :disabled="index === cvData.experience.length - 1" class="p-1 text-slate-400 hover:text-indigo-600 disabled:opacity-30 text-xs">
                                            <i class="fa-solid fa-arrow-down"></i>
                                        </button>
                                        <button @click="removeItem(cvData.experience, index)" class="p-1 text-rose-500 hover:text-rose-700 text-xs">
                                            <i class="fa-solid fa-trash-can"></i>
                                        </button>
                                    </div>
                                </div>

                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                                    <input type="text" v-model="exp.title" placeholder="Job Title" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                    <input type="text" v-model="exp.company" placeholder="Company / Organization" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                    <input type="text" v-model="exp.location" placeholder="Location (e.g. Remote, NY)" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                    <input type="text" v-model="exp.dates" placeholder="Dates (e.g. Jan 2021 - Present)" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                </div>

                                <textarea v-model="exp.description" rows="3" placeholder="Key responsibilities and achievements (separate lines with bullet points)..." class="w-full text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500"></textarea>
                            </div>

                            <button @click="addExperience" class="w-full py-2 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 border border-indigo-200 rounded-lg text-xs font-semibold transition flex items-center justify-center space-x-1">
                                <i class="fa-solid fa-plus text-xs"></i>
                                <span>Add Work Experience</span>
                            </button>
                        </div>
                    </div>

                    <!-- Education Accordion -->
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                        <div @click="toggleAccordion('education')" class="p-4 bg-slate-100/70 border-b border-slate-200 flex items-center justify-between cursor-pointer hover:bg-slate-100">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-md bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-xs">
                                    <i class="fa-solid fa-graduation-cap"></i>
                                </span>
                                <h2 class="font-bold text-slate-800 text-sm">Education ({{ cvData.education.length }})</h2>
                            </div>
                            <i :class="activeAccordion === 'education' ? 'fa-chevron-up' : 'fa-chevron-down'" class="fa-solid text-slate-400 text-xs"></i>
                        </div>

                        <div v-show="activeAccordion === 'education'" class="p-4 space-y-4">
                            <div v-for="(edu, index) in cvData.education" :key="index" class="p-3 bg-slate-50 border border-slate-200 rounded-lg space-y-3">
                                <div class="flex justify-between items-center border-b border-slate-200 pb-2">
                                    <span class="text-xs font-bold text-slate-700">Education #{{ index + 1 }}</span>
                                    <div class="flex items-center space-x-1">
                                        <button @click="moveItem(cvData.education, index, -1)" :disabled="index === 0" class="p-1 text-slate-400 hover:text-indigo-600 disabled:opacity-30 text-xs">
                                            <i class="fa-solid fa-arrow-up"></i>
                                        </button>
                                        <button @click="moveItem(cvData.education, index, 1)" :disabled="index === cvData.education.length - 1" class="p-1 text-slate-400 hover:text-indigo-600 disabled:opacity-30 text-xs">
                                            <i class="fa-solid fa-arrow-down"></i>
                                        </button>
                                        <button @click="removeItem(cvData.education, index)" class="p-1 text-rose-500 hover:text-rose-700 text-xs">
                                            <i class="fa-solid fa-trash-can"></i>
                                        </button>
                                    </div>
                                </div>

                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                                    <input type="text" v-model="edu.degree" placeholder="Degree / Certificate" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                    <input type="text" v-model="edu.institution" placeholder="University / School" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                    <input type="text" v-model="edu.location" placeholder="Location" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                    <input type="text" v-model="edu.dates" placeholder="Graduation Year / Dates" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                </div>

                                <input type="text" v-model="edu.details" placeholder="Honors, GPA, or relevant coursework..." class="w-full text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                            </div>

                            <button @click="addEducation" class="w-full py-2 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 border border-indigo-200 rounded-lg text-xs font-semibold transition flex items-center justify-center space-x-1">
                                <i class="fa-solid fa-plus text-xs"></i>
                                <span>Add Education</span>
                            </button>
                        </div>
                    </div>

                    <!-- Skills Accordion -->
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                        <div @click="toggleAccordion('skills')" class="p-4 bg-slate-100/70 border-b border-slate-200 flex items-center justify-between cursor-pointer hover:bg-slate-100">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-md bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-xs">
                                    <i class="fa-solid fa-code"></i>
                                </span>
                                <h2 class="font-bold text-slate-800 text-sm">Skills & Proficiencies ({{ cvData.skills.length }})</h2>
                            </div>
                            <i :class="activeAccordion === 'skills' ? 'fa-chevron-up' : 'fa-chevron-down'" class="fa-solid text-slate-400 text-xs"></i>
                        </div>

                        <div v-show="activeAccordion === 'skills'" class="p-4 space-y-3">
                            <div class="flex flex-wrap gap-2">
                                <div v-for="(skill, index) in cvData.skills" :key="index" class="flex items-center bg-slate-100 border border-slate-300 rounded-full px-3 py-1 text-xs">
                                    <input type="text" v-model="skill.name" placeholder="Skill Name" class="bg-transparent border-none text-xs text-slate-800 font-medium focus:outline-none w-24">
                                    <button @click="removeItem(cvData.skills, index)" class="ml-1 text-slate-400 hover:text-rose-600">
                                        <i class="fa-solid fa-xmark text-xs"></i>
                                    </button>
                                </div>
                            </div>

                            <button @click="addSkill" class="py-1.5 px-3 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 border border-indigo-200 rounded-lg text-xs font-semibold transition flex items-center space-x-1">
                                <i class="fa-solid fa-plus text-xs"></i>
                                <span>Add Skill</span>
                            </button>
                        </div>
                    </div>

                    <!-- Key Projects Accordion -->
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                        <div @click="toggleAccordion('projects')" class="p-4 bg-slate-100/70 border-b border-slate-200 flex items-center justify-between cursor-pointer hover:bg-slate-100">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-md bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-xs">
                                    <i class="fa-solid fa-diagram-project"></i>
                                </span>
                                <h2 class="font-bold text-slate-800 text-sm">Key Projects ({{ cvData.projects.length }})</h2>
                            </div>
                            <i :class="activeAccordion === 'projects' ? 'fa-chevron-up' : 'fa-chevron-down'" class="fa-solid text-slate-400 text-xs"></i>
                        </div>

                        <div v-show="activeAccordion === 'projects'" class="p-4 space-y-4">
                            <div v-for="(project, index) in cvData.projects" :key="index" class="p-3 bg-slate-50 border border-slate-200 rounded-lg space-y-3">
                                <div class="flex justify-between items-center border-b border-slate-200 pb-2">
                                    <span class="text-xs font-bold text-slate-700">Project #{{ index + 1 }}</span>
                                    <div class="flex items-center space-x-1">
                                        <button @click="moveItem(cvData.projects, index, -1)" :disabled="index === 0" class="p-1 text-slate-400 hover:text-indigo-600 disabled:opacity-30 text-xs">
                                            <i class="fa-solid fa-arrow-up"></i>
                                        </button>
                                        <button @click="moveItem(cvData.projects, index, 1)" :disabled="index === cvData.projects.length - 1" class="p-1 text-slate-400 hover:text-indigo-600 disabled:opacity-30 text-xs">
                                            <i class="fa-solid fa-arrow-down"></i>
                                        </button>
                                        <button @click="removeItem(cvData.projects, index)" class="p-1 text-rose-500 hover:text-rose-700 text-xs">
                                            <i class="fa-solid fa-trash-can"></i>
                                        </button>
                                    </div>
                                </div>

                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                                    <input type="text" v-model="project.name" placeholder="Project Title" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                    <input type="text" v-model="project.link" placeholder="Project Link (URL)" class="text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                </div>
                                <input type="text" v-model="project.technologies" placeholder="Technologies Used (e.g. React, Node.js, AWS)" class="w-full text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                                <textarea v-model="project.description" rows="2" placeholder="Brief project overview & achievements..." class="w-full text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500"></textarea>
                            </div>

                            <button @click="addProject" class="w-full py-2 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 border border-indigo-200 rounded-lg text-xs font-semibold transition flex items-center justify-center space-x-1">
                                <i class="fa-solid fa-plus text-xs"></i>
                                <span>Add Project</span>
                            </button>
                        </div>
                    </div>

                    <!-- Languages & Certifications Accordion -->
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                        <div @click="toggleAccordion('extras')" class="p-4 bg-slate-100/70 border-b border-slate-200 flex items-center justify-between cursor-pointer hover:bg-slate-100">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-md bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-xs">
                                    <i class="fa-solid fa-certificate"></i>
                                </span>
                                <h2 class="font-bold text-slate-800 text-sm">Languages & Certifications</h2>
                            </div>
                            <i :class="activeAccordion === 'extras' ? 'fa-chevron-up' : 'fa-chevron-down'" class="fa-solid text-slate-400 text-xs"></i>
                        </div>

                        <div v-show="activeAccordion === 'extras'" class="p-4 space-y-4">
                            <!-- Languages -->
                            <div>
                                <label class="block text-xs font-semibold text-slate-700 mb-2">Languages</label>
                                <div class="space-y-2">
                                    <div v-for="(lang, index) in cvData.languages" :key="index" class="flex items-center space-x-2">
                                        <input type="text" v-model="lang.name" placeholder="Language" class="w-1/2 text-xs border border-slate-300 rounded p-1.5">
                                        <input type="text" v-model="lang.level" placeholder="Proficiency (e.g. Native, Fluent)" class="w-1/2 text-xs border border-slate-300 rounded p-1.5">
                                        <button @click="removeItem(cvData.languages, index)" class="text-rose-500 hover:text-rose-700 p-1">
                                            <i class="fa-solid fa-xmark"></i>
                                        </button>
                                    </div>
                                    <button @click="addLanguage" class="text-xs text-indigo-600 font-semibold hover:underline flex items-center space-x-1">
                                        <i class="fa-solid fa-plus text-[10px]"></i>
                                        <span>Add Language</span>
                                    </button>
                                </div>
                            </div>

                            <!-- Certifications -->
                            <div class="border-t border-slate-200 pt-3">
                                <label class="block text-xs font-semibold text-slate-700 mb-2">Certifications</label>
                                <div class="space-y-2">
                                    <div v-for="(cert, index) in cvData.certifications" :key="index" class="flex items-center space-x-2">
                                        <input type="text" v-model="cert.title" placeholder="Certification Name" class="w-2/3 text-xs border border-slate-300 rounded p-1.5">
                                        <input type="text" v-model="cert.year" placeholder="Year" class="w-1/3 text-xs border border-slate-300 rounded p-1.5">
                                        <button @click="removeItem(cvData.certifications, index)" class="text-rose-500 hover:text-rose-700 p-1">
                                            <i class="fa-solid fa-xmark"></i>
                                        </button>
                                    </div>
                                    <button @click="addCertification" class="text-xs text-indigo-600 font-semibold hover:underline flex items-center space-x-1">
                                        <i class="fa-solid fa-plus text-[10px]"></i>
                                        <span>Add Certification</span>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Custom Section Accordion -->
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                        <div @click="toggleAccordion('custom')" class="p-4 bg-slate-100/70 border-b border-slate-200 flex items-center justify-between cursor-pointer hover:bg-slate-100">
                            <div class="flex items-center space-x-3">
                                <span class="w-7 h-7 rounded-md bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold text-xs">
                                    <i class="fa-solid fa-asterisk"></i>
                                </span>
                                <h2 class="font-bold text-slate-800 text-sm">Custom Section</h2>
                            </div>
                            <i :class="activeAccordion === 'custom' ? 'fa-chevron-up' : 'fa-chevron-down'" class="fa-solid text-slate-400 text-xs"></i>
                        </div>

                        <div v-show="activeAccordion === 'custom'" class="p-4 space-y-3">
                            <input type="text" v-model="cvData.customSection.title" placeholder="Section Title (e.g. Publications, Volunteer Work)" class="w-full text-xs font-semibold border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500">
                            <textarea v-model="cvData.customSection.content" rows="3" placeholder="Enter content details or bullet points..." class="w-full text-xs border border-slate-300 rounded p-2 focus:ring-1 focus:ring-indigo-500"></textarea>
                        </div>
                    </div>

                </div>
            </section>

            <!-- RIGHT PANE: Live CV Preview -->
            <section v-show="viewMode === 'preview' || viewMode === 'split'" 
                     :class="{'w-full lg:w-1/2': viewMode === 'split', 'w-full': viewMode === 'preview'}" 
                     class="print-container h-full overflow-y-auto custom-scrollbar bg-slate-300/60 p-4 sm:p-8 flex justify-center items-start transition-all duration-300">
                
                <!-- Printable CV Paper Container -->
                <div class="cv-paper rounded-sm overflow-hidden my-0 mx-auto relative transition-all"
                     :style="{ fontFamily: cvData.settings.font + ', sans-serif', fontSize: getFontSizeClass() }">

                    <!-- ========================================== -->
                    <!-- TEMPLATE 1: MODERN TWO-COLUMN             -->
                    <!-- ========================================== -->
                    <div v-if="cvData.settings.template === 'modern'" class="flex flex-row min-h-full">
                        <!-- Left Sidebar Column -->
                        <div class="w-1/3 p-6 text-slate-100 space-y-6 flex-shrink-0" :style="{ backgroundColor: cvData.settings.themeColor }">
                            <!-- Photo -->
                            <div v-if="cvData.personal.photo" class="text-center">
                                <img :src="cvData.personal.photo" alt="Profile" class="w-28 h-28 rounded-full mx-auto border-2 border-white/50 object-cover shadow-md">
                            </div>

                            <!-- Contact Info -->
                            <div class="space-y-2 text-[0.85em] opacity-90 break-words">
                                <h3 class="text-[0.95em] font-bold uppercase tracking-wider border-b border-white/20 pb-1 mb-2">Contact</h3>
                                <p v-if="cvData.personal.email" class="flex items-center space-x-2">
                                    <i class="fa-solid fa-envelope w-4 text-center"></i>
                                    <span>{{ cvData.personal.email }}</span>
                                </p>
                                <p v-if="cvData.personal.phone" class="flex items-center space-x-2">
                                    <i class="fa-solid fa-phone w-4 text-center"></i>
                                    <span>{{ cvData.personal.phone }}</span>
                                </p>
                                <p v-if="cvData.personal.location" class="flex items-center space-x-2">
                                    <i class="fa-solid fa-location-dot w-4 text-center"></i>
                                    <span>{{ cvData.personal.location }}</span>
                                </p>
                                <p v-if="cvData.personal.website" class="flex items-center space-x-2">
                                    <i class="fa-solid fa-globe w-4 text-center"></i>
                                    <span>{{ cvData.personal.website }}</span>
                                </p>
                                <p v-if="cvData.personal.linkedin" class="flex items-center space-x-2">
                                    <i class="fa-brands fa-linkedin w-4 text-center"></i>
                                    <span>{{ cvData.personal.linkedin }}</span>
                                </p>
                                <p v-if="cvData.personal.github" class="flex items-center space-x-2">
                                    <i class="fa-brands fa-github w-4 text-center"></i>
                                    <span>{{ cvData.personal.github }}</span>
                                </p>
                            </div>

                            <!-- Skills -->
                            <div v-if="cvData.skills.length > 0" class="space-y-2">
                                <h3 class="text-[0.95em] font-bold uppercase tracking-wider border-b border-white/20 pb-1 mb-2">Skills</h3>
                                <div class="flex flex-wrap gap-1.5">
                                    <span v-for="s in cvData.skills" :key="s.name" class="bg-white/15 px-2 py-0.5 rounded text-[0.8em]">
                                        {{ s.name }}
                                    </span>
                                </div>
                            </div>

                            <!-- Languages -->
                            <div v-if="cvData.languages.length > 0" class="space-y-2">
                                <h3 class="text-[0.95em] font-bold uppercase tracking-wider border-b border-white/20 pb-1 mb-2">Languages</h3>
                                <div class="space-y-1 text-[0.85em]">
                                    <div v-for="l in cvData.languages" :key="l.name" class="flex justify-between">
                                        <span class="font-medium">{{ l.name }}</span>
                                        <span class="opacity-75">{{ l.level }}</span>
                                    </div>
                                </div>
                            </div>

                            <!-- Certifications -->
                            <div v-if="cvData.certifications.length > 0" class="space-y-2">
                                <h3 class="text-[0.95em] font-bold uppercase tracking-wider border-b border-white/20 pb-1 mb-2">Certifications</h3>
                                <div class="space-y-1 text-[0.85em]">
                                    <div v-for="c in cvData.certifications" :key="c.title">
                                        <p class="font-medium leading-tight">{{ c.title }}</p>
                                        <p class="text-[0.85em] opacity-75">{{ c.year }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Right Main Content -->
                        <div class="w-2/3 p-8 text-slate-800 space-y-6">
                            <!-- Header -->
                            <div>
                                <h1 class="text-3xl font-extrabold tracking-tight" :style="{ color: cvData.settings.themeColor }">
                                    {{ cvData.personal.fullName || 'Your Name' }}
                                </h1>
                                <p class="text-lg font-semibold text-slate-600 mt-0.5">
                                    {{ cvData.personal.jobTitle || 'Professional Title' }}
                                </p>
                            </div>

                            <!-- Summary -->
                            <div v-if="cvData.summary" class="space-y-1.5">
                                <h2 class="text-xs font-bold uppercase tracking-widest text-slate-400">Profile Summary</h2>
                                <p class="text-slate-600 leading-relaxed text-[0.9em] whitespace-pre-line">{{ cvData.summary }}</p>
                            </div>

                            <!-- Work Experience -->
                            <div v-if="cvData.experience.length > 0" class="space-y-3">
                                <h2 class="text-xs font-bold uppercase tracking-widest text-slate-400 border-b border-slate-200 pb-1">Work Experience</h2>
                                <div v-for="(exp, i) in cvData.experience" :key="i" class="space-y-1">
                                    <div class="flex justify-between items-baseline">
                                        <h3 class="font-bold text-slate-800 text-[1em]">{{ exp.title }}</h3>
                                        <span class="text-[0.8em] text-slate-500 font-medium">{{ exp.dates }}</span>
                                    </div>
                                    <div class="text-[0.85em] font-semibold text-slate-600 flex justify-between">
                                        <span>{{ exp.company }}</span>
                                        <span class="text-slate-400 font-normal">{{ exp.location }}</span>
                                    </div>
                                    <p class="text-[0.85em] text-slate-600 leading-relaxed whitespace-pre-line mt-1">{{ exp.description }}</p>
                                </div>
                            </div>

                            <!-- Education -->
                            <div v-if="cvData.education.length > 0" class="space-y-3">
                                <h2 class="text-xs font-bold uppercase tracking-widest text-slate-400 border-b border-slate-200 pb-1">Education</h2>
                                <div v-for="(edu, i) in cvData.education" :key="i" class="space-y-0.5">
                                    <div class="flex justify-between items-baseline">
                                        <h3 class="font-bold text-slate-800 text-[0.95em]">{{ edu.degree }}</h3>
                                        <span class="text-[0.8em] text-slate-500 font-medium">{{ edu.dates }}</span>
                                    </div>
                                    <p class="text-[0.85em] text-slate-600 font-medium">{{ edu.institution }} <span v-if="edu.location" class="text-slate-400 font-normal">| {{ edu.location }}</span></p>
                                    <p v-if="edu.details" class="text-[0.8em] text-slate-500">{{ edu.details }}</p>
                                </div>
                            </div>

                            <!-- Projects -->
                            <div v-if="cvData.projects.length > 0" class="space-y-3">
                                <h2 class="text-xs font-bold uppercase tracking-widest text-slate-400 border-b border-slate-200 pb-1">Key Projects</h2>
                                <div v-for="(proj, i) in cvData.projects" :key="i" class="space-y-1">
                                    <div class="flex justify-between items-baseline">
                                        <h3 class="font-bold text-slate-800 text-[0.9em]">{{ proj.name }}</h3>
                                        <span v-if="proj.link" class="text-[0.8em] text-indigo-600 underline">{{ proj.link }}</span>
                                    </div>
                                    <p v-if="proj.technologies" class="text-[0.8em] font-semibold text-slate-500">Tech: {{ proj.technologies }}</p>
                                    <p class="text-[0.85em] text-slate-600 leading-relaxed">{{ proj.description }}</p>
                                </div>
                            </div>

                            <!-- Custom Section -->
                            <div v-if="cvData.customSection.title" class="space-y-2">
                                <h2 class="text-xs font-bold uppercase tracking-widest text-slate-400 border-b border-slate-200 pb-1">{{ cvData.customSection.title }}</h2>
                                <p class="text-[0.85em] text-slate-600 whitespace-pre-line leading-relaxed">{{ cvData.customSection.content }}</p>
                            </div>
                        </div>
                    </div>


                    <!-- ========================================== -->
                    <!-- TEMPLATE 2: EXECUTIVE CLASSIC             -->
                    <!-- ========================================== -->
                    <div v-else-if="cvData.settings.template === 'executive'" class="p-10 text-slate-900 space-y-6">
                        <!-- Centered Classic Header -->
                        <div class="text-center border-b-2 pb-6" :style="{ borderColor: cvData.settings.themeColor }">
                            <h1 class="text-4xl font-extrabold tracking-tight uppercase" :style="{ color: cvData.settings.themeColor }">
                                {{ cvData.personal.fullName || 'Your Full Name' }}
                            </h1>
                            <p class="text-lg font-medium text-slate-600 mt-1 uppercase tracking-wider">
                                {{ cvData.personal.jobTitle || 'Executive Title' }}
                            </p>
                            
                            <!-- Inline Contact Row -->
                            <div class="flex flex-wrap items-center justify-center gap-3 text-[0.85em] text-slate-600 mt-3 font-medium">
                                <span v-if="cvData.personal.email"><i class="fa-solid fa-envelope mr-1"></i>{{ cvData.personal.email }}</span>
                                <span v-if="cvData.personal.phone">â¢ <i class="fa-solid fa-phone mr-1"></i>{{ cvData.personal.phone }}</span>
                                <span v-if="cvData.personal.location">â¢ <i class="fa-solid fa-location-dot mr-1"></i>{{ cvData.personal.location }}</span>
                                <span v-if="cvData.personal.website">â¢ <i class="fa-solid fa-globe mr-1"></i>{{ cvData.personal.website }}</span>
                                <span v-if="cvData.personal.linkedin">â¢ <i class="fa-brands fa-linkedin mr-1"></i>{{ cvData.personal.linkedin }}</span>
                            </div>
                        </div>

                        <!-- Summary -->
                        <div v-if="cvData.summary" class="space-y-2">
                            <h2 class="text-sm font-bold uppercase tracking-wider border-b border-slate-300 pb-0.5" :style="{ color: cvData.settings.themeColor }">Executive Profile</h2>
                            <p class="text-slate-700 leading-relaxed text-[0.9em] whitespace-pre-line">{{ cvData.summary }}</p>
                        </div>

                        <!-- Experience -->
                        <div v-if="cvData.experience.length > 0" class="space-y-4">
                            <h2 class="text-sm font-bold uppercase tracking-wider border-b border-slate-300 pb-0.5" :style="{ color: cvData.settings.themeColor }">Professional Experience</h2>
                            <div v-for="(exp, i) in cvData.experience" :key="i" class="space-y-1">
                                <div class="flex justify-between items-baseline font-bold">
                                    <span class="text-[0.95em] text-slate-800">{{ exp.title }} â <span class="font-medium text-slate-600">{{ exp.company }}</span></span>
                                    <span class="text-[0.8em] text-slate-500">{{ exp.dates }}</span>
                                </div>
                                <div class="text-[0.8em] text-slate-500 italic">{{ exp.location }}</div>
                                <p class="text-[0.85em] text-slate-700 leading-relaxed whitespace-pre-line mt-1">{{ exp.description }}</p>
                            </div>
                        </div>

                        <!-- Education & Skills 2-Column Grid -->
                        <div class="grid grid-cols-2 gap-6">
                            <!-- Education -->
                            <div v-if="cvData.education.length > 0" class="space-y-3">
                                <h2 class="text-sm font-bold uppercase tracking-wider border-b border-slate-300 pb-0.5" :style="{ color: cvData.settings.themeColor }">Education</h2>
                                <div v-for="(edu, i) in cvData.education" :key="i" class="space-y-0.5">
                                    <h3 class="font-bold text-slate-800 text-[0.9em]">{{ edu.degree }}</h3>
                                    <p class="text-[0.85em] text-slate-600">{{ edu.institution }} ({{ edu.dates }})</p>
                                    <p v-if="edu.details" class="text-[0.8em] text-slate-500 italic">{{ edu.details }}</p>
                                </div>
                            </div>

                            <!-- Skills -->
                            <div v-if="cvData.skills.length > 0" class="space-y-3">
                                <h2 class="text-sm font-bold uppercase tracking-wider border-b border-slate-300 pb-0.5" :style="{ color: cvData.settings.themeColor }">Core Competencies</h2>
                                <div class="flex flex-wrap gap-1.5">
                                    <span v-for="s in cvData.skills" :key="s.name" class="border border-slate-300 px-2 py-0.5 rounded text-[0.8em] font-medium text-slate-700 bg-slate-50">
                                        {{ s.name }}
                                    </span>
                                </div>
                            </div>
                        </div>

                        <!-- Projects & Extras -->
                        <div v-if="cvData.projects.length > 0" class="space-y-3">
                            <h2 class="text-sm font-bold uppercase tracking-wider border-b border-slate-300 pb-0.5" :style="{ color: cvData.settings.themeColor }">Key Initiatives & Projects</h2>
                            <div v-for="(p, i) in cvData.projects" :key="i" class="space-y-1">
                                <div class="flex justify-between font-bold text-[0.9em]">
                                    <span>{{ p.name }}</span>
                                    <span v-if="p.link" class="text-slate-500 font-normal text-[0.8em]">{{ p.link }}</span>
                                </div>
                                <p class="text-[0.85em] text-slate-700">{{ p.description }}</p>
                            </div>
                        </div>

                        <!-- Custom Section -->
                        <div v-if="cvData.customSection.title" class="space-y-2">
                            <h2 class="text-sm font-bold uppercase tracking-wider border-b border-slate-300 pb-0.5" :style="{ color: cvData.settings.themeColor }">{{ cvData.customSection.title }}</h2>
                            <p class="text-[0.85em] text-slate-700 whitespace-pre-line leading-relaxed">{{ cvData.customSection.content }}</p>
                        </div>
                    </div>


                    <!-- ========================================== -->
                    <!-- TEMPLATE 3: SLEEK MINIMALIST              -->
                    <!-- ========================================== -->
                    <div v-else-if="cvData.settings.template === 'minimalist'" class="p-10 text-slate-800 space-y-8">
                        <!-- Top Header -->
                        <div class="flex justify-between items-start border-b pb-6">
                            <div class="space-y-1">
                                <h1 class="text-3xl font-light text-slate-900 tracking-tight">
                                    <span class="font-bold" :style="{ color: cvData.settings.themeColor }">{{ cvData.personal.fullName || 'Your Name' }}</span>
                                </h1>
                                <p class="text-base font-medium text-slate-500 uppercase tracking-widest text-[0.85em]">
                                    {{ cvData.personal.jobTitle || 'Professional Role' }}
                                </p>
                            </div>
                            <div class="text-right text-[0.8em] text-slate-500 space-y-0.5">
                                <p v-if="cvData.personal.email">{{ cvData.personal.email }}</p>
                                <p v-if="cvData.personal.phone">{{ cvData.personal.phone }}</p>
                                <p v-if="cvData.personal.location">{{ cvData.personal.location }}</p>
                                <p v-if="cvData.personal.website">{{ cvData.personal.website }}</p>
                            </div>
                        </div>

                        <!-- Summary -->
                        <div v-if="cvData.summary" class="text-[0.9em] text-slate-600 leading-relaxed italic border-l-2 pl-4" :style="{ borderColor: cvData.settings.themeColor }">
                            {{ cvData.summary }}
                        </div>

                        <!-- Experience -->
                        <div v-if="cvData.experience.length > 0" class="space-y-4">
                            <h2 class="text-xs font-bold uppercase tracking-widest text-slate-400">Experience</h2>
                            <div v-for="(exp, i) in cvData.experience" :key="i" class="space-y-1">
                                <div class="flex justify-between items-baseline">
                                    <h3 class="font-bold text-slate-900 text-[0.95em]">{{ exp.title }}</h3>
                                    <span class="text-[0.75em] text-slate-400 font-mono">{{ exp.dates }}</span>
                                </div>
                                <div class="text-[0.85em] text-indigo-600 font-medium">{{ exp.company }} <span class="text-slate-400 font-normal">/ {{ exp.location }}</span></div>
                                <p class="text-[0.85em] text-slate-600 leading-relaxed whitespace-pre-line pt-1">{{ exp.description }}</p>
                            </div>
                        </div>

                        <!-- Grid for Education & Skills -->
                        <div class="grid grid-cols-2 gap-8">
                            <!-- Education -->
                            <div v-if="cvData.education.length > 0" class="space-y-3">
                                <h2 class="text-xs font-bold uppercase tracking-widest text-slate-400">Education</h2>
                                <div v-for="(edu, i) in cvData.education" :key="i" class="space-y-0.5">
                                    <h3 class="font-bold text-slate-800 text-[0.9em]">{{ edu.degree }}</h3>
                                    <p class="text-[0.8em] text-slate-500">{{ edu.institution }}, {{ edu.dates }}</p>
                                </div>
                            </div>

                            <!-- Skills -->
                            <div v-if="cvData.skills.length > 0" class="space-y-3">
                                <h2 class="text-xs font-bold uppercase tracking-widest text-slate-400">Skills</h2>
                                <p class="text-[0.85em] text-slate-600 leading-relaxed">
                                    <span v-for="(s, i) in cvData.skills" :key="i">
                                        {{ s.name }}<span v-if="i < cvData.skills.length - 1"> â¢ </span>
                                    </span>
                                </p>
                            </div>
                        </div>

                        <!-- Projects -->
                        <div v-if="cvData.projects.length > 0" class="space-y-3">
                            <h2 class="text-xs font-bold uppercase tracking-widest text-slate-400">Projects</h2>
                            <div v-for="(proj, i) in cvData.projects" :key="i" class="space-y-1">
                                <div class="flex justify-between text-[0.9em]">
                                    <span class="font-bold text-slate-800">{{ proj.name }}</span>
                                    <span v-if="proj.link" class="text-slate-400 text-[0.8em]">{{ proj.link }}</span>
                                </div>
                                <p class="text-[0.85em] text-slate-600">{{ proj.description }}</p>
                            </div>
                        </div>
                    </div>


                    <!-- ========================================== -->
                    <!-- TEMPLATE 4: SIDEBAR ACCENT                -->
                    <!-- ========================================== -->
                    <div v-else-if="cvData.settings.template === 'sidebar-accent'" class="flex min-h-full">
                        <!-- Thick Left Accent Line -->
                        <div class="w-3 flex-shrink-0" :style="{ backgroundColor: cvData.settings.themeColor }"></div>

                        <div class="p-8 flex-1 space-y-6">
                            <!-- Header with Optional Photo -->
                            <div class="flex justify-between items-center border-b pb-4">
                                <div>
                                    <h1 class="text-3xl font-black tracking-tight text-slate-900">
                                        {{ cvData.personal.fullName || 'Your Name' }}
                                    </h1>
                                    <p class="text-lg font-semibold" :style="{ color: cvData.settings.themeColor }">
                                        {{ cvData.personal.jobTitle || 'Professional Role' }}
                                    </p>
                                </div>
                                <img v-if="cvData.personal.photo" :src="cvData.personal.photo" class="w-20 h-20 rounded-lg object-cover shadow-sm border">
                            </div>

                            <!-- Contacts Bar -->
                            <div class="flex flex-wrap gap-4 text-[0.8em] text-slate-600 bg-slate-50 p-2.5 rounded-lg border border-slate-100">
                                <span v-if="cvData.personal.email"><i class="fa-solid fa-envelope mr-1 text-slate-400"></i>{{ cvData.personal.email }}</span>
                                <span v-if="cvData.personal.phone"><i class="fa-solid fa-phone mr-1 text-slate-400"></i>{{ cvData.personal.phone }}</span>
                                <span v-if="cvData.personal.location"><i class="fa-solid fa-location-dot mr-1 text-slate-400"></i>{{ cvData.personal.location }}</span>
                                <span v-if="cvData.personal.website"><i class="fa-solid fa-globe mr-1 text-slate-400"></i>{{ cvData.personal.website }}</span>
                            </div>

                            <!-- Summary -->
                            <div v-if="cvData.summary" class="space-y-1">
                                <h2 class="text-xs font-bold uppercase tracking-wider text-slate-400">About Me</h2>
                                <p class="text-[0.85em] text-slate-600 leading-relaxed whitespace-pre-line">{{ cvData.summary }}</p>
                            </div>

                            <!-- Experience -->
                            <div v-if="cvData.experience.length > 0" class="space-y-3">
                                <h2 class="text-xs font-bold uppercase tracking-wider text-slate-400">Work Experience</h2>
                                <div v-for="(exp, i) in cvData.experience" :key="i" class="border-l-2 pl-3 space-y-1" :style="{ borderColor: cvData.settings.themeColor }">
                                    <div class="flex justify-between font-bold text-[0.9em]">
                                        <span>{{ exp.title }}</span>
                                        <span class="text-[0.8em] text-slate-400 font-normal">{{ exp.dates }}</span>
                                    </div>
                                    <div class="text-[0.8em] text-slate-600 font-medium">{{ exp.company }} â {{ exp.location }}</div>
                                    <p class="text-[0.85em] text-slate-600 whitespace-pre-line">{{ exp.description }}</p>
                                </div>
                            </div>

                            <!-- Education & Skills -->
                            <div class="grid grid-cols-2 gap-6">
                                <div v-if="cvData.education.length > 0" class="space-y-2">
                                    <h2 class="text-xs font-bold uppercase tracking-wider text-slate-400">Education</h2>
                                    <div v-for="(edu, i) in cvData.education" :key="i" class="text-[0.85em]">
                                        <div class="font-bold text-slate-800">{{ edu.degree }}</div>
                                        <div class="text-slate-500">{{ edu.institution }} ({{ edu.dates }})</div>
                                    </div>
                                </div>

                                <div v-if="cvData.skills.length > 0" class="space-y-2">
                                    <h2 class="text-xs font-bold uppercase tracking-wider text-slate-400">Key Skills</h2>
                                    <div class="flex flex-wrap gap-1">
                                        <span v-for="s in cvData.skills" :key="s.name" class="bg-slate-100 px-2 py-0.5 rounded text-[0.75em] text-slate-700">
                                            {{ s.name }}
                                        </span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                </div>
            </section>
        </main>
    </div>

    <script>
        const { createApp } = Vue;

        createApp({
            data() {
                return {
                    viewMode: 'split', // 'edit', 'preview', 'split'
                    activeAccordion: 'personal',
                    showShareModal: false,
                    shareableUrl: '',
                    copyButtonText: 'Copy Link',
                    toastMessage: '',
                    isSharedView: false,

                    // Preset themes
                    themeColors: [
                        { name: 'Navy Blue', hex: '#1e3a8a' },
                        { name: 'Emerald Green', hex: '#065f46' },
                        { name: 'Slate Gray', hex: '#334155' },
                        { name: 'Deep Indigo', hex: '#3730a3' },
                        { name: 'Crimson Red', hex: '#991b1b' },
                        { name: 'Charcoal', hex: '#1f2937' }
                    ],

                    // Templates
                    templates: [
                        { id: 'modern', name: 'Two-Column' },
                        { id: 'executive', name: 'Executive' },
                        { id: 'minimalist', name: 'Minimalist' },
                        { id: 'sidebar-accent', name: 'Accent Sidebar' }
                    ],

                    // Main reactive CV Data state
                    cvData: {
                        settings: {
                            template: 'modern',
                            themeColor: '#1e3a8a',
                            font: 'Inter',
                            fontSize: 'normal',
                            spacing: 'normal'
                        },
                        personal: {
                            fullName: 'Alexandra Chen',
                            jobTitle: 'Senior Full-Stack Software Engineer',
                            email: 'alexandra.chen@example.com',
                            phone: '+1 (555) 382-9102',
                            location: 'San Francisco, CA',
                            website: 'alexandrachen.dev',
                            linkedin: 'linkedin.com/in/alexchen',
                            github: 'github.com/alexchen-dev',
                            photo: ''
                        },
                        summary: 'Passionate Senior Full-Stack Engineer with 8+ years of experience designing, scaling, and deploying cloud-native web applications. Proven track record of leading cross-functional engineering teams, optimizing modern frontend architectures, and reducing backend API latency by 40%.',
                        experience: [
                            {
                                title: 'Lead Full-Stack Engineer',
                                company: 'CloudPulse Systems',
                                location: 'San Francisco, CA',
                                dates: 'Jan 2022 - Present',
                                description: 'â¢ Architected micro-frontend systems using React, TypeScript, and Tailwind CSS serving 2M+ active users.\nâ¢ Reduced database query load by 35% through Redis caching strategies and GraphQL batching.\nâ¢ Mentored a team of 6 junior and mid-level software engineers.'
                            },
                            {
                                title: 'Senior Software Developer',
                                company: 'Apex Digital Solutions',
                                location: 'Austin, TX',
                                dates: 'Mar 2018 - Dec 2021',
                                description: 'â¢ Built real-time analytics dashboard features using Node.js, WebSockets, and Vue.js.\nâ¢ Integrated AWS S3 and Lambda pipelines for automated user document processing.'
                            }
                        ],
                        education: [
                            {
                                degree: 'B.S. in Computer Science',
                                institution: 'Stanford University',
                                location: 'Stanford, CA',
                                dates: '2014 - 2018',
                                details: 'Graduated Magna Cum Laude â¢ GPA: 3.92/4.0'
                            }
                        ],
                        skills: [
                            { name: 'JavaScript (ES6+)' },
                            { name: 'TypeScript' },
                            { name: 'React.js' },
                            { name: 'Vue.js' },
                            { name: 'Node.js' },
                            { name: 'Python' },
                            { name: 'GraphQL' },
                            { name: 'Docker' },
                            { name: 'AWS' },
                            { name: 'Tailwind CSS' }
                        ],
                        projects: [
                            {
                                name: 'DevFlow - Developer Productivity Suite',
                                link: 'github.com/alexchen/devflow',
                                technologies: 'React, Node.js, PostgreSQL, Tailwind',
                                description: 'An open-source browser extension and workflow automated dashboard for GitHub PR tracking and ticket management.'
                            }
                        ],
                        languages: [
                            { name: 'English', level: 'Native / Fluent' },
                            { name: 'Spanish', level: 'Intermediate' }
                        ],
                        certifications: [
                            { title: 'AWS Certified Solutions Architect', year: '2023' },
                            { title: 'Certified Kubernetes Administrator (CKA)', year: '2022' }
                        ],
                        customSection: {
                            title: 'Publications & Speaking',
                            content: 'â¢ Keynote Speaker at TechCon 2023: "Scaling Real-time Micro-frontends"\nâ¢ Author of "Modern Web Architecture Patterns" (Published on Dev.to, 50k+ readers)'
                        }
                    }
                };
            },

            mounted() {
                // 1. Check if URL contains an encoded share link in the hash
                const hash = window.location.hash;
                if (hash && hash.includes('cv=')) {
                    try {
                        const encodedData = hash.split('cv=')[1];
                        const jsonString = LZString.decompressFromEncodedURIComponent(encodedData);
                        if (jsonString) {
                            this.cvData = JSON.parse(jsonString);
                            this.isSharedView = true;
                            this.showToast('Loaded CV from shared link!');
                        }
                    } catch (e) {
                        console.error('Failed to parse URL shared CV data', e);
                    }
                } else {
                    // 2. Load cached CV data from LocalStorage if available
                    const savedData = localStorage.getItem('procv_user_data');
                    if (savedData) {
                        try {
                            this.cvData = JSON.parse(savedData);
                        } catch (e) {
                            console.error('Failed to load saved CV state', e);
                        }
                    }
                }

                // Adjust default view mode for smaller mobile screens
                if (window.innerWidth < 1024) {
                    this.viewMode = 'edit';
                }
            },

            watch: {
                // Auto-save changes to LocalStorage
                cvData: {
                    deep: true,
                    handler(newData) {
                        localStorage.setItem('procv_user_data', JSON.stringify(newData));
                    }
                }
            },

            methods: {
                toggleAccordion(section) {
                    this.activeAccordion = this.activeAccordion === section ? null : section;
                },

                getFontSizeClass() {
                    switch (this.cvData.settings.fontSize) {
                        case 'small': return '13px';
                        case 'large': return '16px';
                        default: return '14.5px';
                    }
                },

                showToast(msg) {
                    this.toastMessage = msg;
                    setTimeout(() => {
                        this.toastMessage = '';
                    }, 3500);
                },

                // Share Link Methods
                generateShareLink() {
                    const jsonString = JSON.stringify(this.cvData);
                    const compressed = LZString.compressToEncodedURIComponent(jsonString);
                    const baseUrl = window.location.origin + window.location.pathname;
                    this.shareableUrl = `${baseUrl}#cv=${compressed}`;
                    this.copyButtonText = 'Copy Link';
                    this.showShareModal = true;
                },

                copyShareUrl() {
                    const textarea = document.createElement('textarea');
                    textarea.value = this.shareableUrl;
                    document.body.appendChild(textarea);
                    textarea.select();
                    document.execCommand('copy');
                    document.body.removeChild(textarea);

                    this.copyButtonText = 'Copied!';
                    this.showToast('Shareable link copied to clipboard!');
                    setTimeout(() => {
                        this.copyButtonText = 'Copy Link';
                    }, 2000);
                },

                clearUrlHash() {
                    this.isSharedView = false;
                    history.replaceState(null, null, window.location.pathname);
                    this.showToast('Cleared shared link parameter from URL');
                },

                // Photo Upload Handler
                uploadPhoto(event) {
                    const file = event.target.files[0];
                    if (file) {
                        const reader = new FileReader();
                        reader.onload = (e) => {
                            this.cvData.personal.photo = e.target.result;
                        };
                        reader.readAsDataURL(file);
                    }
                },

                // Dynamic List Handlers
                addExperience() {
                    this.cvData.experience.push({
                        title: '', company: '', location: '', dates: '', description: ''
                    });
                },

                addEducation() {
                    this.cvData.education.push({
                        degree: '', institution: '', location: '', dates: '', details: ''
                    });
                },

                addSkill() {
                    this.cvData.skills.push({ name: '' });
                },

                addProject() {
                    this.cvData.projects.push({
                        name: '', link: '', technologies: '', description: ''
                    });
                },

                addLanguage() {
                    this.cvData.languages.push({ name: '', level: '' });
                },

                addCertification() {
                    this.cvData.certifications.push({ title: '', year: '' });
                },

                removeItem(list, index) {
                    list.splice(index, 1);
                },

                moveItem(list, index, direction) {
                    const targetIndex = index + direction;
                    if (targetIndex >= 0 && targetIndex < list.length) {
                        const temp = list[index];
                        list[index] = list[targetIndex];
                        list[targetIndex] = temp;
                    }
                },

                // Sample Data Preloader
                loadSampleData() {
                    this.cvData = {
                        settings: {
                            template: 'modern',
                            themeColor: '#1e3a8a',
                            font: 'Inter',
                            fontSize: 'normal',
                            spacing: 'normal'
                        },
                        personal: {
                            fullName: 'Alexandra Chen',
                            jobTitle: 'Senior Full-Stack Software Engineer',
                            email: 'alexandra.chen@example.com',
                            phone: '+1 (555) 382-9102',
                            location: 'San Francisco, CA',
                            website: 'alexandrachen.dev',
                            linkedin: 'linkedin.com/in/alexchen',
                            github: 'github.com/alexchen-dev',
                            photo: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=300&auto=format&fit=crop&q=80'
                        },
                        summary: 'Passionate Senior Full-Stack Engineer with 8+ years of experience designing, scaling, and deploying cloud-native web applications. Proven track record of leading cross-functional engineering teams, optimizing modern frontend architectures, and reducing backend API latency by 40%.',
                        experience: [
                            {
                                title: 'Lead Full-Stack Engineer',
                                company: 'CloudPulse Systems',
                                location: 'San Francisco, CA',
                                dates: 'Jan 2022 - Present',
                                description: 'â¢ Architected micro-frontend systems using React, TypeScript, and Tailwind CSS serving 2M+ active users.\nâ¢ Reduced database query load by 35% through Redis caching strategies and GraphQL batching.\nâ¢ Mentored a team of 6 junior and mid-level software engineers.'
                            },
                            {
                                title: 'Senior Software Developer',
                                company: 'Apex Digital Solutions',
                                location: 'Austin, TX',
                                dates: 'Mar 2018 - Dec 2021',
                                description: 'â¢ Built real-time analytics dashboard features using Node.js, WebSockets, and Vue.js.\nâ¢ Integrated AWS S3 and Lambda pipelines for automated user document processing.'
                            }
                        ],
                        education: [
                            {
                                degree: 'B.S. in Computer Science',
                                institution: 'Stanford University',
                                location: 'Stanford, CA',
                                dates: '2014 - 2018',
                                details: 'Graduated Magna Cum Laude â¢ GPA: 3.92/4.0'
                            }
                        ],
                        skills: [
                            { name: 'JavaScript' },
                            { name: 'TypeScript' },
                            { name: 'React.js' },
                            { name: 'Vue.js' },
                            { name: 'Node.js' },
                            { name: 'Python' },
                            { name: 'GraphQL' },
                            { name: 'AWS' }
                        ],
                        projects: [
                            {
                                name: 'DevFlow Productivity Tool',
                                link: 'github.com/alexchen/devflow',
                                technologies: 'React, Node.js, PostgreSQL',
                                description: 'An open-source browser plugin for GitHub PR tracking and developer metrics analysis.'
                            }
                        ],
                        languages: [
                            { name: 'English', level: 'Native / Fluent' },
                            { name: 'Spanish', level: 'Intermediate' }
                        ],
                        certifications: [
                            { title: 'AWS Solutions Architect', year: '2023' }
                        ],
                        customSection: {
                            title: 'Publications & Speaking',
                            content: 'â¢ Keynote Speaker at TechCon 2023: "Scaling Micro-frontends"'
                        }
                    };
                    this.showToast('Sample data loaded!');
                },

                // Reset Form
                clearForm() {
                    this.cvData.personal = { fullName: '', jobTitle: '', email: '', phone: '', location: '', website: '', linkedin: '', github: '', photo: '' };
                    this.cvData.summary = '';
                    this.cvData.experience = [];
                    this.cvData.education = [];
                    this.cvData.skills = [];
                    this.cvData.projects = [];
                    this.cvData.languages = [];
                    this.cvData.certifications = [];
                    this.cvData.customSection = { title: '', content: '' };
                    localStorage.removeItem('procv_user_data');
                    this.showToast('Form cleared!');
                },

                // Export Config JSON File
                exportJSON() {
                    const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(this.cvData, null, 2));
                    const downloadAnchor = document.createElement('a');
                    downloadAnchor.setAttribute("href", dataStr);
                    downloadAnchor.setAttribute("download", `CV_${this.cvData.personal.fullName.replace(/\s+/g, '_') || 'Resume'}.json`);
                    document.body.appendChild(downloadAnchor);
                    downloadAnchor.click();
                    downloadAnchor.remove();
                },

                // Import Config JSON File
                importJSON(event) {
                    const file = event.target.files[0];
                    if (file) {
                        const reader = new FileReader();
                        reader.onload = (e) => {
                            try {
                                const importedData = JSON.parse(e.target.result);
                                this.cvData = importedData;
                                this.showToast('Resume configuration imported!');
                            } catch (err) {
                                this.showToast('Error parsing JSON file');
                            }
                        };
                        reader.readAsText(file);
                    }
                },

                // Trigger Print Dialog
                printCV() {
                    const previousMode = this.viewMode;
                    if (this.viewMode === 'edit') {
                        this.viewMode = 'preview';
                    }
                    setTimeout(() => {
                        window.print();
                        this.viewMode = previousMode;
                    }, 200);
                }
            }
        }).mount('#app');
    </script>
</body>
</html>
