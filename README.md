# SecurePass Landing Page

## Project Analysis & Overview

### What is SecurePass?

**SecurePass** is a professional-grade password manager application that combines security, usability, and reliability. It's a desktop application built with modern .NET technology that helps users securely store and manage sensitive passwords and credentials.

---

## Project Architecture

### Core Components

#### 1. **SecurePass.Core** - Cryptography & Vault Management
The foundation of the application providing:

- **CryptoService.cs**: Handles all cryptographic operations
  - AES-256-GCM encryption for data protection
  - PBKDF2 key derivation with SHA-256 (100,000 iterations)
  - Cryptographically secure random number generation
  - Encryption/Decryption of vault data

- **VaultService.cs**: Manages vault lifecycle
  - Creates new vaults with master password
  - Loads and unlocks vaults
  - Saves encrypted vault files
  - Handles recovery key operations
  - Manages password entries

- **Models.cs**: Data structures
  - `PasswordEntry`: Individual password records with metadata
  - `VaultData`: Container for all password entries and settings
  - `VaultFile`: Encrypted vault file format with headers
  - `HeaderInfo`: Encryption metadata and key material
  - `UserSettings`: User preferences (auto-lock duration, etc.)

#### 2. **SecurePass.UI** - Windows Desktop Application
The user interface built with WPF:

- **MainWindow.xaml/cs**: Primary application window
  - Password list management
  - Auto-lock functionality (default 5 minutes)
  - Real-time session timer
  - Search and filter capabilities

- **Views/** - Multiple specialized screens:
  - `LoginView`: Authentication and vault unlocking
  - `SetupView`: Initial vault creation
  - `ManageSecurePassView`: Main password vault interface
  - `AddEditPasswordView`: Create/edit password entries
  - `ChangePasswordView`: Modify master password
  - `RecoveryView`: Recovery key management

- **Styles/AppStyles.xaml**: Centralized styling and theming
- **WindowHelper.cs**: UI utilities and window management

---

## Security Architecture

### Dual-Key Protection System

SecurePass uses an innovative **dual-key encryption model**:

```
┌─────────────────────────────────────────────────┐
│         File Key (Actual Encryption Key)         │
│              (256-bit random)                    │
└─────────────────────────────────────────────────┘
          ↙                               ↖
    [Master Password]              [Recovery Key]
         ↓                               ↓
    [PBKDF2]                       [PBKDF2]
     SHA-256                        SHA-256
   100,000 iter                   100,000 iter
         ↓                               ↓
   Master Key                    Recovery Key
     (256-bit)                    (256-bit)
         ↓                               ↓
   [AES-256-GCM Encrypt]    [AES-256-GCM Encrypt]
         ↓                               ↓
 MainKeyEncrypted          RecoveryKeyEncrypted
   (60 bytes)                   (60 bytes)
```

### Encryption Standards

- **Algorithm**: AES-256-GCM (Advanced Encryption Standard with Galois/Counter Mode)
- **Key Size**: 256-bit (32 bytes)
- **Nonce Size**: 96-bit (12 bytes) - unique for each encryption
- **Authentication Tag**: 128-bit (16 bytes) - ensures integrity
- **Key Derivation**: PBKDF2 with SHA-256, 100,000 iterations

### Vault File Format

```
┌──────────────────────────────────┐
│     VaultFile Header             │
├──────────────────────────────────┤
│ MasterKeySalt (16 bytes)         │
│ RecoveryKeySalt (16 bytes)       │
│ MainKeyEncrypted (60 bytes)      │
│ RecoveryKeyEncrypted (60 bytes)  │
│ DataNonce (12 bytes)             │
├──────────────────────────────────┤
│   EncryptedData (variable)       │
│   (All passwords + settings)     │
└──────────────────────────────────┘
```

---

## Key Features

### 🔒 Military-Grade Encryption
- AES-256-GCM encryption ensures data remains protected even if the vault file is stolen
- Authentication tags prevent tampering

### 🔑 Recovery Keys
- Generate human-readable recovery keys during vault creation
- Allows account recovery if master password is forgotten
- Recovery keys are equally protected with encryption

### ⏱️ Auto-Lock Protection
- Automatic vault locking after configurable inactivity period
- Default: 5 minutes
- Customizable per user preferences
- Visual countdown timer

### 💾 Secure Local Storage
- No cloud dependencies
- Full user control over data
- Encrypted vault files stored locally
- Offline-first design

### 🔄 Password Management
- Add, edit, delete password entries
- Store metadata: website, username, notes, timestamps
- Search and filter capabilities
- Track creation and modification dates

### ⚙️ Customizable Settings
- Adjust auto-lock duration
- Change master password
- Manage recovery keys
- User preferences storage

---

## Technology Stack

- **Framework**: .NET 8.0 - Modern, long-term supported framework
- **UI Framework**: Windows Presentation Foundation (WPF) - Rich desktop UI
- **Cryptography**: System.Security.Cryptography - Built-in .NET crypto library
- **Serialization**: System.Text.Json - Efficient JSON serialization
- **Language**: C# - Modern, type-safe programming language
- **Architecture**: Modular separation of concerns (Core + UI)

---

## Workflow & User Journey

### 1. Setup (First Run)
- User launches application
- Enters master password (with confirmation)
- System generates recovery key and vault file
- Recovery key displayed and stored

### 2. Daily Usage
- User launches app and enters master password
- Vault unlocks and displays password list
- Auto-lock timer starts (default 5 minutes)
- User can add, edit, or view password entries
- Each action resets the inactivity timer

### 3. Recovery Scenario
- User forgets master password
- Clicks "Recovery" option
- Enters recovery key
- Can reset master password or access vault directly
- No data loss occurs

### 4. Security Scenarios
- **Session Timeout**: Vault auto-locks, requires password re-entry
- **Data Integrity**: GCM mode detects any tampering
- **Brute Force Protection**: PBKDF2 with 100,000 iterations makes password attacks slow

---

## Landing Page Features

The landing page (`index.html`) showcases:

### 📱 Responsive Design
- Mobile-first approach
- Works on desktop, tablet, and mobile devices
- Smooth scrolling navigation
- Hamburger menu support

### 🎨 Modern UI/UX
- Gradient backgrounds and smooth animations
- Interactive feature cards with hover effects
- Sticky navigation bar
- Professional color scheme

### 📄 Content Sections
1. **Navigation** - Quick links to all sections
2. **Hero Section** - Eye-catching project introduction
3. **Features** - 6 key features with icons
4. **Security Architecture** - Detailed security explanation
5. **Technical Details** - Step-by-step workflow
6. **About** - Technology stack and project info
7. **Footer** - Links and copyright

### ✨ Interactive Elements
- Smooth scroll animations
- Hover effects on cards and buttons
- Intersection Observer for fade-in animations
- Active link highlighting
- CTA button interactions

---

## File Structure

```
SecurePassApp/
├── SecurePass.Core/
│   ├── CryptoService.cs
│   ├── Models.cs
│   ├── VaultService.cs
│   └── SecurePass.Core.csproj
├── SecurePass.UI/
│   ├── MainWindow.xaml
│   ├── MainWindow.xaml.cs
│   ├── App.xaml
│   ├── App.xaml.cs
│   ├── WindowHelper.cs
│   ├── Views/
│   │   ├── LoginView.xaml
│   │   ├── SetupView.xaml
│   │   ├── ManageSecurePassView.xaml
│   │   ├── AddEditPasswordView.xaml
│   │   ├── ChangePasswordView.xaml
│   │   └── RecoveryView.xaml
│   ├── Styles/
│   │   └── AppStyles.xaml
│   ├── ViewModels/
│   │   └── ViewModelBase.cs
│   └── SecurePass.UI.csproj
├── Landing/                    ← NEW
│   ├── index.html
│   ├── styles.css
│   ├── script.js
│   └── README.md
└── SecurePass.sln
```

---

## How to View the Landing Page

1. Navigate to the `Landing` folder
2. Open `index.html` in any modern web browser
3. Explore the interactive landing page showcasing SecurePass features

### Browser Compatibility
- Chrome/Chromium (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

---

## Customization

### Colors & Branding
Edit the CSS variables in `styles.css`:
```css
:root {
    --primary-color: #2c3e50;
    --secondary-color: #3498db;
    --accent-color: #e74c3c;
    /* ... more colors ... */
}
```

### Content
Edit `index.html` to update:
- Feature descriptions
- Security details
- About section
- Contact information

### Animations
Modify `script.js` to adjust:
- Scroll animations
- Hover effects
- Intersection Observer behavior

---

## Security Considerations

### What SecurePass Protects
✅ Password data (encrypted at rest and in transit)
✅ Vault file integrity (GCM authentication)
✅ Key material (secure random generation)
✅ Session data (auto-lock protection)

### What Users Should Do
- Use a strong master password (12+ characters, mixed case, numbers, symbols)
- Store recovery keys in a safe location
- Keep the vault file backed up securely
- Use the application on trusted computers only

---

## Future Enhancement Ideas

1. **Multi-platform Support**: Extend to Linux, macOS, mobile
2. **Cloud Sync**: Optional encrypted cloud backup
3. **Password Generator**: Built-in strong password generation
4. **Breach Alerts**: Notify if passwords appear in data breaches
5. **Import/Export**: Support for migrating from other password managers
6. **Two-Factor Authentication**: Additional security layer
7. **Sharing**: Secure password sharing between users
8. **Audit Logs**: Track all vault access and modifications

---

## Conclusion

SecurePass represents a commitment to security, usability, and user trust. By combining industry-standard cryptography with a clean, intuitive interface, it provides a compelling alternative to cloud-based password managers for users who want complete control over their sensitive data.

The landing page effectively communicates these values and features to potential users, emphasizing both security and ease of use.

---

**Created**: February 2026
**Status**: Complete
**Next Steps**: Deploy landing page to web hosting, market to target audience
