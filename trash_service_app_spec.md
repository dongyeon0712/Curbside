# Trash Service Marketplace - App Specification

## 1. CORE FEATURES

### 1.1 Job Management
- **Job Creation**
  - Homeowner sets address/location
  - Selects trash type(s): Trash, Recycling, Compost, Other
  - Sets recurring schedule: Weekly on specific days (e.g., Sunday out / Monday in)
  - Sets custom instructions/notes
  - Uploads reference photos (location to grab from, where to put)
  - Sets pay amount per job

- **Job Posting**
  - Visible to helpers in the area
  - Shows proximity (distance from helper's location)
  - Job status: Open, Accepted, In Progress, Completed, Paid

- **Job Acceptance**
  - Helper accepts the job for a specific date
  - Only one helper can accept per job date
  - Helper gets confirmation notification

### 1.2 Photo Verification
- **Before/After System**
  - Helper takes photo of completed job (trash in or out)
  - Photo includes timestamp and location metadata
  - Homeowner can review photo before approving payment
  - Dispute resolution if photo doesn't match expectations

- **Photo Requirements**
  - Minimum quality standards (not blurry, properly lit)
  - Timestamp embedded
  - GPS/location confirmation

### 1.3 Payment System
- **Payment Flow**
  1. Homeowner adds payment method (credit card, bank transfer)
  2. Helper accepts job
  3. Helper completes job & submits photo
  4. Homeowner approves or disputes
  5. Funds transfer to helper's account
  6. Helper withdraws to Venmo (or other platform)

- **Pricing**
  - Homeowner sets per-job rate
  - Suggested pricing range (e.g., $15-$50 depending on location)
  - Recurring discount option (if booked for multiple weeks)

- **Payment Methods**
  - Homeowner: Credit card, debit card, bank account
  - Helper: Bank account (for app wallet), Venmo integration

- **Withdrawal & Payouts**
  - Minimum withdrawal amount (e.g., $5)
  - Withdrawal processing time (e.g., 1-3 business days)
  - Venmo API integration or manual Venmo link

---

## 2. USER ROLES & FLOWS

### 2.1 HOMEOWNER FLOW

**Signup/Onboarding**
- Email/phone verification
- Profile setup (name, profile photo)
- Payment method registration
- Address verification

**Creating a Job**
1. Input home address
2. Upload photos:
   - "Where to grab trash" photo
   - "Where to put trash" photo (same location, out vs. in)
3. Select trash types
4. Set schedule (weekly, specific days/times)
5. Write custom instructions (e.g., "Don't block driveway")
6. Set pay rate
7. Choose access method (e.g., gate code, front porch)
8. Review and post

**Managing Jobs**
- View active jobs and helpers assigned
- Update instructions anytime
- Change pay rate for future jobs
- Pause/resume recurring job
- Cancel job (with refund to helper if applicable)

**Approving Work**
- Receive notification when helper submits photo
- Review photo evidence
- Approve & release payment OR dispute
- Rate helper (1-5 stars, comment)
- Provide feedback

**Payment**
- View payment history
- Adjust payment method
- Download invoice/receipts

---

### 2.2 HELPER FLOW

**Signup/Onboarding**
- Email/phone verification
- Profile setup (name, profile photo)
- Background check consent (if required)
- Bank account for payout
- Location/service area selection
- Availability (which days/times available)

**Finding Jobs**
- Browse nearby jobs (map view + list view)
- Filter by:
  - Distance from current location
  - Pay rate
  - Schedule (Sundays only, etc.)
  - Trash type
- View full job details:
  - Reference photos
  - Instructions
  - Homeowner rating
  - Pay amount

**Accepting Jobs**
- Accept specific job instance or recurring job
- Receive confirmation with:
  - Exact address
  - Gate code/access instructions
  - Reference photos
  - Contact method for homeowner

**Completing Jobs**
1. Navigate to address
2. View reference photos
3. Complete task (put trash out or bring in)
4. Take completion photo
5. Submit photo
6. Mark as complete

**Payment**
- View account balance
- View earnings history (per-job breakdown)
- Withdraw to bank account
- Link Venmo for fast transfers
- Rate homeowner

**Rating/Reviews**
- Rate homeowner (1-5 stars, comment)
- View own ratings and reviews

---

## 3. TECHNICAL REQUIREMENTS

### 3.1 Frontend
- Mobile app (iOS/Android) - recommended for field work
- Web dashboard (both users)
- Real-time notifications

### 3.2 Backend
- User authentication (email/SMS)
- Payment processing (Stripe, Square, etc.)
- Photo storage & verification (AWS S3, Firebase)
- Location services (geocoding, proximity calculation)
- Job scheduling & recurring job logic
- Rating/review system
- Notification system (push, email, SMS)

### 3.3 Third-Party Integrations
- Payment processor (Stripe, Square)
- Venmo API (for direct transfers)
- Maps API (Google Maps, Mapbox)
- SMS/Push notifications (Twilio, Firebase)
- Identity verification (Trulioo, Jumio)

### 3.4 Data Requirements
- User profile data
- Address/location data
- Job data
- Photo metadata (timestamp, GPS)
- Payment/transaction records
- Rating/review data

---

## 4. ACCESS & SECURITY

### 4.1 Access Methods
- Gate code
- Door code/keypad
- Front porch access (no code needed)
- Neighbor pickup (helper picks up from neighbor)
- Key box (combination lock)

**Implementation:** Homeowner selects access type during job creation, provides code/instructions in app (encrypted)

### 4.2 Verification
- SMS/Email verification for both users
- Photo metadata verification (timestamp, location)
- Background check for helpers (optional but recommended)

### 4.3 Data Security
- End-to-end encryption for sensitive data (codes, payment info)
- PCI compliance for payment processing
- GDPR compliance for location/personal data
- Secure photo storage with deletion after X days

---

## 5. PAYMENT FLOW (DETAILED)

```
Homeowner Perspective:
1. Add payment method (one-time or saved)
2. Post job with price ($X per job)
3. Helper accepts job
4. Helper completes & submits photo
5. Homeowner reviews photo → Approve or Dispute
6. [If Approved] Payment charged to Homeowner's card
7. Funds appear in Helper's app wallet

Helper Perspective:
1. Accept job
2. Complete task
3. Submit photo
4. Wait for homeowner approval
5. Funds appear in wallet
6. Withdraw to bank account or Venmo
```

### 5.1 Pricing Strategy Options
- **Fixed Price:** Homeowner sets fixed rate (e.g., $25/job)
- **Tiered Pricing:** By distance, trash type, extra services
- **Surge Pricing:** Higher rate during holidays/bad weather
- **Subscription:** Homeowner pays weekly/monthly flat fee for recurring service

### 5.2 Commission/Platform Fee
- Decide: Does platform take 5-20% of each transaction?
- Or: Flat fee per transaction?
- Or: Freemium (free for first X jobs)?

### 5.3 Refund Policy
- Homeowner cancels before job: Full refund
- Helper doesn't show up: Full refund to homeowner, helper account flagged
- Disputed photo: Platform reviews, decides refund or payment proceeds

---

## 6. RELIABILITY & ACCOUNTABILITY

### 6.1 Ratings & Reviews
- **Homeowner Ratings (from helpers):** 1-5 stars + comment
  - Are you communicative?
  - Is the job clear?
  - Do you pay on time?
  
- **Helper Ratings (from homeowners):** 1-5 stars + comment
  - Did they show up on time?
  - Did they follow instructions?
  - Quality of work?
  - Trustworthiness?

- **Helper Rating Threshold:** Jobs only visible to helpers if they have 4+ stars or are new (0 jobs)
- **Homeowner Cancellation Rate:** Tracked; if too high, account flagged

### 6.2 No-Show Protocol
- **Helper doesn't show up:**
  1. Homeowner can report after agreed time
  2. Helper gets 1 strike
  3. After 3 strikes: Account suspended
  4. Homeowner gets refund

- **Homeowner doesn't approve payment:**
  1. Photo disputed
  2. Platform mediates (reviews photo quality, matches instructions)
  3. If legitimate: Helper gets paid, Homeowner keeps funds
  4. Pattern of disputes: Homeowner account flagged

### 6.3 Cancellation Policy
- **Homeowner cancels:**
  - More than 48 hours before: Full refund + Helper can find replacement job
  - Less than 48 hours: 50% fee to Helper as compensation, or full refund if Helper didn't accept yet

- **Helper cancels:**
  - After accepting: 24-hour window to cancel free
  - Less than 24 hours before: Marked as "no-show," affects rating

---

## 7. EDGE CASES & SPECIAL HANDLING

### 7.1 Multiple Trash Types
- Job can include: Trash + Recycling + Compost
- Photos show location of each type
- Instructions may differ per type

### 7.2 Damaged/Missing Trash Can
- Homeowner can report damage after job completion
- Photo evidence required
- Platform decides: Helper reimbursement, refund, or both parties split cost

### 7.3 Weather & Accessibility
- Helper can request rescheduling due to weather
- Homeowner can cancel/reschedule if weather severe
- Icy driveway → photo shows effort made, reasonable standard

### 7.4 Holidays
- Homeowner can mark dates as holiday (trash day might change)
- Post special holiday jobs
- Higher pay option for holiday jobs

### 7.5 Recurring Job Modifications
- Homeowner can pause specific week (e.g., on vacation)
- Pause for multiple weeks
- Increase/decrease pay for future jobs
- Helper can opt-out of future instances

---

## 8. LEGAL & COMPLIANCE

### 8.1 Terms of Service
- User obligations
- Platform liability limitations
- Dispute resolution process
- Account termination conditions

### 8.2 Privacy Policy
- Data collection (location, photos, payment)
- Data retention (delete photos after X days?)
- GDPR compliance (EU users)
- CCPA compliance (California users)

### 8.3 Liability & Insurance
- Helper liability waiver
- Homeowner acknowledges third-party access
- Platform not responsible for theft/damage (unless negligent)
- Consider liability insurance for premium tier

### 8.4 Background Checks
- Optional for MVP, required for scaled version
- Third-party service (Checkr, Triplebyte, etc.)
- Disqualifying offenses (theft, trespassing, etc.)

### 8.5 Tax Compliance
- 1099 tax forms for helpers (income above threshold)
- Platform reports to IRS (Form 1099-K)
- Homeowner can report payment as household service expense

---

## 9. QUALITY ASSURANCE & DISPUTE RESOLUTION

### 9.1 Photo Verification
**Checklist for Homeowner/Platform:**
- ✓ Photo timestamp matches job time
- ✓ Location metadata matches job address
- ✓ Trash cans visible and in correct position
- ✓ Photo isn't blurry/dark
- ✓ No obvious damage or issues

### 9.2 Dispute Process
1. Homeowner disputes photo within 24 hours of submission
2. Helper gets 24 hours to respond or resubmit photo
3. If not resolved: Platform team manually reviews
4. Platform decides: Payment proceeds or refund issued
5. Repeated disputes: Account flagged

### 9.3 Escalation
- Platform mediates disputes (trained support team)
- Arbitration clause in ToS
- Maximum refund guaranteed

---

## 10. BUSINESS METRICS & TRACKING

### 10.1 For Homeowners
- Jobs completed on time %
- Helper satisfaction score
- Total money spent
- Upcoming jobs

### 10.2 For Helpers
- Jobs completed %
- Earnings this week/month
- Rating score
- Job availability in area

### 10.3 For Platform
- Total GMV (Gross Merchandise Volume)
- Number of active homeowners/helpers
- Job completion rate
- Average rating
- Dispute rate
- User retention

---

## 11. ROADMAP (FUTURE FEATURES)

**Phase 1 (MVP):**
- Job posting & acceptance
- Photo submission
- Basic payments (fixed price)
- Simple rating system

**Phase 2:**
- Recurring jobs scheduling
- Integration with Venmo
- Advanced search/filtering
- Helper availability calendar

**Phase 3:**
- Background checks
- Premium tier (no platform fees, priority)
- Additional services (yard work, snow removal)
- Subscription model for homeowners
- Multi-property support

**Phase 4:**
- AI photo verification (auto-checks if trash in correct spot)
- Predictive scheduling (recommend optimal helpers)
- Homeowner app for reminders
- SMS/push notifications

---

## 12. SUCCESS CRITERIA

✓ User signup flow < 3 minutes  
✓ Job creation < 5 minutes  
✓ Photo upload/payment < 2 minutes  
✓ 95%+ job completion rate  
✓ <5% dispute rate  
✓ 4.5+ average rating (both users)  
✓ Zero fraudulent transactions  
