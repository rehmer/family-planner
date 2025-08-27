# Family Weekend Planner - Technical Documentation

## Overview

The Family Weekend Planner is a web application that allows family members to coordinate their availability for weekends. It provides a shared calendar interface where each person can mark themselves as "available" or "unavailable" for specific weekend dates, with real-time synchronization across all devices.

## How It Works

### Core Functionality
1. **Family Member Management**: Add/remove family members who will be using the planner
2. **Personal Selection**: Each person selects their name and marks their availability on weekend dates
3. **Visual Status Indicators**: 
   - Individual border colors show each person's selection
   - Background colors show group status (Green = everyone available, Red = someone unavailable, Yellow = mixed)
4. **Real-time Synchronization**: Changes are immediately saved and shared with all family members
5. **Persistent Storage**: All data persists across browser sessions and device changes

### User Interaction Flow
1. Family members visit the shared URL
2. Add all family member names (done once by first person)
3. Each person selects their name from dropdown
4. Click on weekend dates to cycle through: blank → available → unavailable → blank
5. Changes auto-save and sync to all other users in real-time

## Technical Architecture

### Frontend Technology
- **Pure HTML/CSS/JavaScript**: No frameworks required, runs in any modern browser
- **ES6 Modules**: Uses modern JavaScript import syntax for Firebase integration
- **Responsive Design**: Works on desktop, tablet, and mobile devices
- **Event-Driven UI**: Uses DOM event listeners for user interactions

### Backend Technology
- **Firebase Realtime Database**: Google's cloud-hosted NoSQL database
- **Real-time Synchronization**: Firebase automatically pushes changes to all connected clients
- **No Server Required**: Completely serverless architecture

### Data Structure
```javascript
{
  "plannerState": {
    "people": ["Mom", "Dad", "Son", "Daughter"],
    "selections": {
      "2025-08-30": {
        "Mom": "available",
        "Dad": "unavailable",
        "Son": "available"
      },
      "2025-09-07": {
        "Mom": "available",
        "Dad": "available",
        "Son": "available",
        "Daughter": "available"
      }
    }
  }
}
```

### Firebase Configuration
- **Project**: `weekend-planner-b469f`
- **Database URL**: `https://weekend-planner-b469f-default-rtdb.firebaseio.com/`
- **Security Rules**: Currently set to allow public read/write access
- **Authentication**: None required (suitable for trusted family sharing)

## Hosting & Deployment

### Current Setup
- **Hosting**: GitHub Pages (free static hosting)
- **Domain**: `https://[username].github.io/family-planner/`
- **Deployment**: Automatic - any push to main branch updates live site
- **SSL**: Provided automatically by GitHub Pages

### Files Structure
```
/
├── index.html          # Main application file (contains all HTML, CSS, JS)
├── family-photo.jpg    # Header photo (optional)
└── README.md          # This documentation
```

## Data Persistence Strategy

### Primary Storage: Firebase Realtime Database
- Real-time synchronization across all devices
- Automatic conflict resolution
- Offline capability with sync when reconnected
- 99.95% uptime SLA from Google

### Backup Storage: Browser localStorage
- Local fallback if Firebase is unavailable
- Preserves data during temporary network issues
- Automatically syncs with Firebase when connection restored

### User Session Persistence
- Selected user name stored in browser's localStorage
- Remembers which family member you are across sessions
- No login required - based on device/browser

## Maintenance Requirements

### Immediate (Within 30 days)
Firebase is currently in "test mode" which expires automatically. To maintain functionality:

1. Go to Firebase Console → Realtime Database → Rules
2. Replace current rules with:
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
3. Click "Publish"

### Long-term
- **No ongoing maintenance required** once rules are updated
- Firebase handles all scaling, backups, and infrastructure
- GitHub Pages hosting is maintenance-free
- Consider reviewing Firebase usage/costs annually (likely to remain in free tier)

## Security Considerations

### Current Security Model
- **Open Access**: Anyone with the URL can read/write data
- **Suitable For**: Trusted family sharing scenario
- **Risk Level**: Low (no sensitive data, limited user base)

### Alternative Security Options
If enhanced security is needed in the future:
1. **Authentication**: Add Firebase Auth (email/password or Google sign-in)
2. **Access Control**: Restrict write access to authenticated users only
3. **Private Sharing**: Use Firebase user management for invitation-only access

## Cost Analysis

### Current Costs: $0
- Firebase: Well within free tier limits (50GB/month, 20K simultaneous connections)
- GitHub Pages: Free for public repositories
- Domain: Using GitHub's subdomain (free)

### Projected Costs
- **Ongoing**: $0/month (usage will remain in free tiers)
- **Scale**: Could handle 100+ family members before any costs
- **Backup Plan**: If Firebase costs arise, could migrate to other free databases

## Troubleshooting

### Common Issues
1. **"Loading Planner..." persists**: Check Firebase rules and browser console for errors
2. **Changes not syncing**: Verify internet connection and Firebase project status
3. **Data loss**: Check localStorage backup via browser developer tools

### Debug Features
- Built-in debug panel (click "Show Debug" button)
- Browser console logging for all operations
- Firebase status monitoring through debug interface

## Future Enhancement Ideas

### Potential Features
- Email notifications for fully available weekends
- Calendar integration (Google Calendar, Outlook)
- Weather integration for weekend planning
- Activity suggestions based on availability
- Historical data analysis (most popular weekends, etc.)

### Technical Improvements
- Progressive Web App (PWA) capabilities for offline use
- Push notifications for availability changes
- Export functionality (PDF, iCal format)
- Custom date ranges beyond just weekends

## Recovery & Backup

### Data Recovery
- Firebase maintains automatic backups
- localStorage provides local backup copy
- Data can be manually exported from Firebase Console if needed

### Disaster Recovery
1. **Firebase Account Loss**: Data can be migrated to new Firebase project
2. **GitHub Account Loss**: Code can be redeployed to any static hosting service
3. **Complete Loss**: Local localStorage copies can be used to rebuild data

---

## Quick Setup Summary for Future Reference

1. **Clone repository** to new GitHub account
2. **Enable GitHub Pages** on the repository
3. **Create new Firebase project** with Realtime Database
4. **Update Firebase config** in index.html with new project details
5. **Set database rules** to allow public read/write
6. **Upload family photo** (optional)
7. **Share URL** with family members

Total setup time: ~15 minutes