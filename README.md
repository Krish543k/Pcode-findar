<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="theme-color" content="#667eea">
    <meta name="description" content="PCode Finder - Product data management with QR/Barcode scanner">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="PCode Finder">
    <link rel="manifest" href="manifest.json">
    <title>PCode Finder</title>
    <script src="https://cdn.jsdelivr.net/npm/html5-qrcode@2.3.8/html5-qrcode.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    transition: background 0.3s ease;
}

body.dark-mode {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
}

body.dark-mode .container,
body.dark-mode .search-section,
body.dark-mode .product-card,
body.dark-mode .modal-content,
body.dark-mode .side-menu {
    background: #0f3460;
    color: #e0e0e0;
}

body.dark-mode .data-label,
body.dark-mode .data-value,
body.dark-mode h2,
body.dark-mode h3,
body.dark-mode h4 {
    color: #e0e0e0;
}

body.dark-mode .data-row {
    border-bottom-color: #1a1a2e;
}

body.dark-mode #searchInput,
body.dark-mode .modal-content input {
    background: #1a1a2e;
    color: #e0e0e0;
    border-color: #667eea;
}

body.dark-mode .toggle-btn:not(.active) {
    background: #1a1a2e;
    color: #999;
}

body.dark-mode .menu-item {
    border-bottom-color: #1a1a2e;
}

body.dark-mode .menu-item span {
    color: #e0e0e0;
}

header {
    background: #667eea;
    color: white;
    padding: 15px 20px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.2);
    position: sticky;
    top: 0;
    z-index: 100;
}

.header-content {
    display: flex;
    align-items: center;
    gap: 15px;
    position: relative;
}

.menu-icon {
    cursor: pointer;
    display: flex;
    flex-direction: column;
    gap: 4px;
    padding: 8px;
    transition: transform 0.3s;
}

.menu-icon:hover {
    transform: scale(1.1);
}

.menu-icon .dot {
    width: 6px;
    height: 6px;
    background: white;
    border-radius: 50%;
}

.dark-mode-toggle {
    position: absolute;
    right: 20px;
    top: 50%;
    transform: translateY(-50%);
    cursor: pointer;
    font-size: 24px;
    transition: transform 0.3s;
}

.dark-mode-toggle:hover {
    transform: translateY(-50%) scale(1.2);
}

h1 {
    font-size: 20px;
    font-weight: 600;
}

.side-menu {
    position: fixed;
    left: -300px;
    top: 0;
    width: 280px;
    height: 100%;
    background: white;
    box-shadow: 2px 0 10px rgba(0,0,0,0.3);
    transition: left 0.3s ease;
    z-index: 1000;
}

.side-menu.active {
    left: 0;
}

.menu-header {
    background: #667eea;
    color: white;
    padding: 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.menu-header h2 {
    font-size: 20px;
    color: white;
}

.close-btn {
    font-size: 30px;
    cursor: pointer;
    line-height: 1;
}

.menu-content {
    padding: 10px 0;
}

.menu-item {
    padding: 15px 20px;
    cursor: pointer;
    border-bottom: 1px solid #eee;
    transition: all 0.3s;
}

.menu-item:hover {
    background: #667eea;
    color: white;
}

.menu-item:hover span {
    color: white;
}

.menu-item span {
    font-size: 16px;
    color: #333;
}

.container {
    max-width: 1200px;
    margin: 20px auto;
    padding: 0 20px;
}

.search-section {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    margin-bottom: 20px;
    display: flex;
    gap: 10px;
}

#searchInput {
    flex: 1;
    padding: 12px;
    border: 2px solid #667eea;
    border-radius: 5px;
    font-size: 16px;
}

.search-section button {
    padding: 12px 24px;
    background: #667eea;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.3s;
}

.search-section button:hover {
    background: #5568d3;
}

.camera-btn {
    padding: 12px 20px;
    background: #4CAF50;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 20px;
    transition: background 0.3s;
}

.camera-btn:hover {
    background: #45a049;
}

.clear-btn {
    padding: 12px 20px;
    background: #f44336;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.3s;
}

.clear-btn:hover {
    background: #da190b;
}

.filter-toggle-btn {
    padding: 12px 20px;
    background: #FF9800;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.3s;
}

.filter-toggle-btn:hover {
    background: #F57C00;
}

.filter-panel {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    margin-bottom: 20px;
    animation: slideInUp 0.3s ease-out;
}

body.dark-mode .filter-panel {
    background: #0f3460;
    color: #e0e0e0;
}

.filter-panel h3 {
    margin-bottom: 15px;
    color: #667eea;
}

body.dark-mode .filter-panel h3 {
    color: #8b9cff;
}

.filter-row {
    display: flex;
    gap: 10px;
    margin-bottom: 15px;
    align-items: center;
    flex-wrap: wrap;
}

.filter-row label {
    font-weight: 600;
    min-width: 100px;
}

.filter-row input,
.filter-row select {
    flex: 1;
    padding: 10px;
    border: 2px solid #667eea;
    border-radius: 5px;
    font-size: 14px;
    min-width: 120px;
}

body.dark-mode .filter-row input,
body.dark-mode .filter-row select {
    background: #1a1a2e;
    color: #e0e0e0;
    border-color: #667eea;
}

.filter-buttons {
    display: flex;
    gap: 10px;
    margin-top: 15px;
}

.filter-buttons button {
    flex: 1;
    padding: 12px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 14px;
    transition: background 0.3s;
}

.filter-buttons button:first-child {
    background: #4CAF50;
    color: white;
}

.filter-buttons button:first-child:hover {
    background: #45a049;
}

.filter-buttons button:last-child {
    background: #f44336;
    color: white;
}

.filter-buttons button:last-child:hover {
    background: #da190b;
}

/* Smooth Animations */
@keyframes slideInUp {
    from {
        opacity: 0;
        transform: translateY(30px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes fadeInScale {
    from {
        opacity: 0;
        transform: scale(0.9);
    }
    to {
        opacity: 1;
        transform: scale(1);
    }
}

.results-container {
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.product-card {
    background: white;
    border-radius: 15px;
    box-shadow: 0 5px 20px rgba(0,0,0,0.15);
    overflow: hidden;
    animation: slideInUp 0.4s ease-out;
}

.product-card.compact {
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    margin-bottom: 15px;
}

.product-card.compact .section-content {
    padding: 15px;
}

.product-card.compact .data-row {
    padding: 6px 0;
    font-size: 14px;
}

.product-card.compact .data-label {
    font-size: 13px;
}

.product-card.compact .data-value {
    font-size: 13px;
}

.product-card.compact .pcode-highlight {
    font-size: 16px;
}

.product-card.compact h4 {
    font-size: 14px;
    margin-bottom: 8px;
}

.product-card.compact .primary-label,
.product-card.compact .secondary-label {
    font-size: 12px;
    padding: 4px 12px;
    margin-bottom: 10px;
}

.product-card.compact .toggle-btn {
    padding: 10px;
    font-size: 14px;
}

.product-card.compact .edit-card-btn,
.product-card.compact .delete-card-btn,
.product-card.compact .share-card-btn {
    padding: 6px 12px;
    font-size: 13px;
    margin-top: 10px;
}

@keyframes slideIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.section-toggle {
    display: flex;
    background: #f5f5f5;
    border-radius: 10px 10px 0 0;
    overflow: hidden;
}

.toggle-btn {
    flex: 1;
    padding: 15px;
    text-align: center;
    cursor: pointer;
    font-weight: 600;
    font-size: 16px;
    transition: all 0.3s;
    position: relative;
}

.toggle-btn.primary {
    background: #667eea;
    color: white;
}

.toggle-btn.secondary {
    background: #764ba2;
    color: white;
}

.toggle-btn:not(.active) {
    background: #e0e0e0;
    color: #666;
}

.toggle-btn.active::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 3px;
    background: white;
}

.section-content {
    padding: 25px;
    display: none;
    animation: fadeIn 0.4s ease;
}

.section-content.active {
    display: block;
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateX(-10px);
    }
    to {
        opacity: 1;
        transform: translateX(0);
    }
}

.primary-section {
    background: linear-gradient(135deg, #667eea15 0%, #667eea05 100%);
}

.primary-label {
    display: inline-block;
    background: #667eea;
    color: white;
    padding: 5px 15px;
    border-radius: 20px;
    font-size: 14px;
    font-weight: 600;
    margin-bottom: 15px;
    animation: pulse 2s infinite;
}

@keyframes pulse {
    0%, 100% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.05);
    }
}

.secondary-section {
    background: linear-gradient(135deg, #764ba215 0%, #764ba205 100%);
}

.secondary-label {
    display: inline-block;
    background: #764ba2;
    color: white;
    padding: 5px 15px;
    border-radius: 20px;
    font-size: 14px;
    font-weight: 600;
    margin-bottom: 15px;
    animation: pulse 2s infinite;
}

.data-row {
    display: flex;
    justify-content: space-between;
    padding: 10px 0;
    border-bottom: 1px solid #eee;
}

.data-row:last-child {
    border-bottom: none;
}

.data-label {
    font-weight: 600;
    color: #555;
}

.data-value {
    color: #333;
    font-weight: 500;
}

.pcode-highlight {
    font-size: 20px;
    color: #667eea;
    font-weight: 700;
}

.delete-card-btn {
    background: #f44336;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 14px;
    margin-top: 15px;
    width: 48%;
    display: inline-block;
    transition: background 0.3s;
}

.delete-card-btn:hover {
    background: #da190b;
}

.edit-card-btn {
    background: #2196F3;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 14px;
    margin-top: 15px;
    width: 48%;
    display: inline-block;
    margin-right: 2%;
    transition: background 0.3s;
}

.edit-card-btn:hover {
    background: #1976D2;
}

.share-card-btn {
    background: #4CAF50;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 14px;
    margin-top: 10px;
    width: 100%;
    transition: background 0.3s;
}

.share-card-btn:hover {
    background: #45a049;
}

body.dark-mode .delete-card-btn {
    background: #d32f2f;
}

body.dark-mode .delete-card-btn:hover {
    background: #b71c1c;
}

/* Password Screen */
.password-screen {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
}

.password-box {
    background: white;
    padding: 40px;
    border-radius: 15px;
    box-shadow: 0 10px 40px rgba(0,0,0,0.3);
    text-align: center;
    max-width: 400px;
    width: 90%;
}

.password-box h2 {
    color: #667eea;
    margin-bottom: 10px;
}

.password-box p {
    color: #666;
    margin-bottom: 20px;
}

.password-box input {
    width: 100%;
    padding: 15px;
    border: 2px solid #667eea;
    border-radius: 5px;
    font-size: 16px;
    margin-bottom: 15px;
}

.password-box button {
    width: 100%;
    padding: 15px;
    background: #667eea;
    color: white;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
}

.password-box button:hover {
    background: #5568d3;
}

.password-error {
    color: #f44336;
    margin-top: 10px;
    font-size: 14px;
}

.lock-icon {
    font-size: 60px;
    margin-bottom: 20px;
}

.price-old {
    text-decoration: line-through;
    color: #999;
}

.price-new {
    color: #4CAF50;
    font-weight: 700;
}

.no-data {
    text-align: center;
    color: #999;
    padding: 60px 20px;
    font-size: 18px;
}

.modal {
    display: none;
    position: fixed;
    z-index: 2000;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.5);
}

.modal.active {
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal-content {
    background: white;
    padding: 30px;
    border-radius: 10px;
    width: 90%;
    max-width: 500px;
    max-height: 80vh;
    overflow-y: auto;
    position: relative;
    animation: fadeInScale 0.3s ease-out;
}

@keyframes modalSlide {
    from {
        transform: translateY(-50px);
        opacity: 0;
    }
    to {
        transform: translateY(0);
        opacity: 1;
    }
}

.modal-content .close {
    position: absolute;
    right: 15px;
    top: 15px;
    font-size: 28px;
    cursor: pointer;
    color: #999;
}

.modal-content h2 {
    margin-bottom: 20px;
    color: #333;
}

.modal-content h3 {
    margin-bottom: 10px;
    color: #667eea;
    font-size: 16px;
}

body.dark-mode .modal-content h3 {
    color: #8b9cff;
}

.modal-content input[type="file"],
.modal-content input[type="text"] {
    width: 100%;
    padding: 12px;
    margin-bottom: 15px;
    border: 2px solid #ddd;
    border-radius: 5px;
    font-size: 14px;
}

.modal-content input[type="text"]:focus {
    border-color: #667eea;
    outline: none;
}

.modal-content button {
    width: 100%;
    padding: 12px;
    background: #667eea;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.3s;
}

.modal-content button:hover {
    background: #5568d3;
}

.success-popup {
    display: none;
    position: fixed;
    z-index: 3000;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}

.success-popup.active {
    display: block;
    animation: popupFade 0.3s ease;
}

@keyframes popupFade {
    from {
        opacity: 0;
        transform: translate(-50%, -50%) scale(0.8);
    }
    to {
        opacity: 1;
        transform: translate(-50%, -50%) scale(1);
    }
}

.popup-content {
    background: white;
    padding: 40px;
    border-radius: 15px;
    box-shadow: 0 10px 40px rgba(0,0,0,0.3);
    text-align: center;
    min-width: 300px;
}

.success-icon {
    width: 80px;
    height: 80px;
    background: #4CAF50;
    color: white;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 50px;
    margin: 0 auto 20px;
    animation: successBounce 0.6s ease;
}

@keyframes successBounce {
    0%, 100% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.2);
    }
}

.popup-content h3 {
    color: #4CAF50;
    margin-bottom: 10px;
}

.popup-content p {
    color: #666;
}

.popup-content p {
    color: #666;
}

.theme-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
    gap: 15px;
    margin-top: 20px;
}

.theme-option {
    cursor: pointer;
    text-align: center;
    transition: transform 0.3s;
}

.theme-option:hover {
    transform: scale(1.05);
}

.theme-preview {
    width: 100%;
    height: 80px;
    border-radius: 10px;
    margin-bottom: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.2);
}

.theme-option p {
    font-size: 14px;
    font-weight: 600;
    color: #333;
}

body.dark-mode .theme-option p {
    color: #e0e0e0;
}

.color-picker-section {
    margin: 20px 0;
}

.color-picker-row {
    display: flex;
    align-items: center;
    gap: 15px;
    margin-bottom: 15px;
    padding: 10px;
    background: #f5f5f5;
    border-radius: 8px;
}

body.dark-mode .color-picker-row {
    background: #1a1a2e;
}

.color-picker-row label {
    font-weight: 600;
    min-width: 130px;
    color: #333;
}

body.dark-mode .color-picker-row label {
    color: #e0e0e0;
}

.color-picker-row input[type="color"] {
    width: 60px;
    height: 40px;
    border: 2px solid #ddd;
    border-radius: 5px;
    cursor: pointer;
}

.color-picker-row span {
    font-family: monospace;
    font-size: 14px;
    color: #666;
}

body.dark-mode .color-picker-row span {
    color: #aaa;
}

.theme-preview-box {
    margin: 20px 0;
    padding: 30px;
    border-radius: 10px;
    text-align: center;
    font-weight: 600;
    font-size: 18px;
    color: white;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.language-tabs {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
    border-bottom: 2px solid #eee;
}

body.dark-mode .language-tabs {
    border-bottom-color: #333;
}

.lang-tab {
    padding: 10px 20px;
    background: transparent;
    border: none;
    border-bottom: 3px solid transparent;
    cursor: pointer;
    font-size: 16px;
    font-weight: 600;
    color: #666;
    transition: all 0.3s;
}

.lang-tab.active {
    color: #667eea;
    border-bottom-color: #667eea;
}

body.dark-mode .lang-tab {
    color: #999;
}

body.dark-mode .lang-tab.active {
    color: #8b9cff;
    border-bottom-color: #8b9cff;
}

.guide-content {
    max-height: 60vh;
    overflow-y: auto;
    padding: 20px;
    background: #f9f9f9;
    border-radius: 10px;
    line-height: 1.8;
}

body.dark-mode .guide-content {
    background: #1a1a2e;
    color: #e0e0e0;
}

.guide-content h3 {
    color: #667eea;
    margin-top: 20px;
    margin-bottom: 10px;
}

body.dark-mode .guide-content h3 {
    color: #8b9cff;
}

.guide-content ul {
    margin-left: 20px;
    margin-bottom: 15px;
}

.guide-content li {
    margin-bottom: 8px;
}

.guide-content code {
    background: #e0e0e0;
    padding: 2px 6px;
    border-radius: 3px;
    font-family: monospace;
}

body.dark-mode .guide-content code {
    background: #0f3460;
}

.zoom-controls {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
    margin-top: 20px;
    padding: 15px;
    background: #f5f5f5;
    border-radius: 10px;
}

body.dark-mode .zoom-controls {
    background: #1a1a2e;
}

.zoom-btn {
    padding: 10px 20px;
    background: #667eea;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 18px;
    transition: all 0.3s;
    min-width: 60px;
}

.zoom-btn:hover {
    background: #5568d3;
    transform: scale(1.05);
}

.zoom-btn:active {
    transform: scale(0.95);
}

#zoomLevel {
    font-size: 18px;
    font-weight: 600;
    color: #667eea;
    min-width: 60px;
    text-align: center;
}

body.dark-mode #zoomLevel {
    color: #8b9cff;
}

.scanner-modal {
    max-width: 600px !important;
}

.scanner-controls {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
    margin: 25px 0;
    padding: 20px;
    background: linear-gradient(135deg, #f5f5f5 0%, #e8e8e8 100%);
    border-radius: 15px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

body.dark-mode .scanner-controls {
    background: linear-gradient(135deg, #1a1a2e 0%, #0f1419 100%);
}

.scanner-btn {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 5px;
    padding: 15px 25px;
    background: #667eea;
    color: white;
    border: none;
    border
