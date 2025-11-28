# Design Guidelines: Security Honeypot Monitoring Dashboard

## Design Approach
**System-Based Approach**: Material Design for data-heavy security monitoring applications
**References**: Datadog, Grafana, Splunk monitoring interfaces
**Principle**: Clarity, scannability, and rapid threat identification

## Typography System

**Primary Font**: Inter via Google Fonts CDN
**Hierarchy**:
- Dashboard Title: text-2xl font-bold
- Section Headers: text-lg font-semibold
- Data Labels: text-sm font-medium uppercase tracking-wide
- Log Entries: text-sm font-mono (for IP addresses, timestamps, URLs)
- Body Text: text-base
- Metadata/Timestamps: text-xs

## Layout System

**Spacing Primitives**: Tailwind units of 2, 4, 6, and 8
- Component padding: p-4, p-6
- Section spacing: space-y-6, space-y-8
- Grid gaps: gap-4, gap-6
- Margins: m-2, m-4, m-8

**Container Structure**:
- Max width: max-w-7xl mx-auto
- Full-width dashboard with sidebar navigation (w-64 fixed)
- Main content area: flex-1 with p-6 to p-8

## Core Components

### Dashboard Layout
- **Sidebar Navigation**: Fixed left sidebar (w-64) with logo, menu items, status indicators
- **Top Bar**: Sticky header with real-time status, notification count, user profile
- **Main Grid**: 12-column responsive grid for stat cards and data tables

### Stat Cards (Overview Metrics)
- Grid layout: grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6
- Each card: Rounded borders (rounded-lg), shadow-sm, p-6
- Content: Large metric number (text-3xl font-bold), label (text-sm), trend indicator
- Icons: Material Icons for metric types (security, notifications, scanning)

### Log Table (Primary Component)
- Full-width responsive table with sticky header
- Columns: Timestamp, IP Address, Request Path, Method, Threat Level, Action
- Row structure: py-3 px-4, hover states for interactivity
- Monospace font for technical data (IP, paths)
- Pagination controls at bottom (showing 50 entries per page)
- Search/filter bar above table with date range picker

### Alert/Notification Cards
- Prominent placement at top of dashboard
- Structure: Icon + Title + Description + Timestamp + Dismiss button
- Severity indicators: Icon variations for different threat levels
- Dismissible with smooth transitions

### Real-time Activity Feed
- Right sidebar (w-80) or dedicated section
- Live scrolling feed of recent scanning attempts
- Compact card format: Icon + Brief description + Time ago
- Auto-scroll with pause on hover

### Telegram Integration Status
- Status indicator component in top bar
- Shows connection status, last notification sent, bot status
- Quick settings access button

### Detailed Threat View (Modal/Slide-over)
- Triggered when clicking log entry
- Full request details: Headers, payload, geolocation
- Threat analysis summary
- Action buttons: Block IP, Create Rule, Send to Telegram

## Navigation Structure

**Sidebar Menu Items**:
1. Dashboard (overview)
2. Live Monitoring
3. Threat Log
4. Blocked IPs
5. Telegram Settings
6. Rules & Filters

## Component Specifications

### Data Visualization
- Simple bar charts for scan attempts over time (using Chart.js)
- Geographic map showing attack origins (Leaflet.js)
- Threat type distribution (pie/donut chart)

### Forms (Telegram Settings)
- Input fields: rounded-md, border, px-4 py-2
- Labels: text-sm font-medium mb-2
- Grouped sections with clear headings
- Save button: Primary action style, px-6 py-3

### Badges & Tags
- Pill-shaped: rounded-full px-3 py-1 text-xs font-medium
- Use for threat levels, request methods, status indicators
- Consistent sizing across application

## Icons
**Library**: Material Icons via CDN
**Usage**:
- Navigation: 24px icons
- Stat cards: 32px icons
- Table rows: 16px icons
- Buttons: 20px icons with text

## Responsive Behavior

**Desktop (lg+)**: Full sidebar + main content + optional right panel
**Tablet (md)**: Collapsible sidebar, 2-column stat grid
**Mobile**: Hidden sidebar (hamburger menu), single-column layout, stacked cards

## Animations
**Minimal Use Only**:
- Table row hover: Subtle background transition
- New log entry: Brief highlight fade (1s)
- Real-time notifications: Slide-in from top
- No scroll animations, no complex transitions

## Accessibility
- ARIA labels for all interactive elements
- Keyboard navigation for tables and forms
- Focus indicators on all interactive elements
- Screen reader announcements for new threats
- High contrast text throughout

## Images
No hero images required. This is a functional dashboard.
**Optional**: Small logo/branding in sidebar header (max 48px height)

---

**Critical Implementation Notes**:
- Prioritize data density over white space
- Ensure table scrolls smoothly with large datasets
- Maintain consistent monospace formatting for technical data
- Real-time updates should be non-intrusive
- All timestamps in user's timezone with clear formatting