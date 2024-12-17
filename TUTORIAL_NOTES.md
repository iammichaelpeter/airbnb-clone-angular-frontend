
0. Frontend Setup
  - npm installations
  - layout
  - shared
  - state
  - auth


📱 **BACKEND ZU FRONTEND IMPLEMENTATION**
1. Proxy & Backend Integration
  * proxy.conf.mjs erstellen:
    ```javascript
    export default [{
      context: ['/api', '/oauth2', '/login', '/assets'],
      target: 'http://localhost:8080',
      secure: true
    }]
    ```
    - Leitet spezifische Pfade an Backend weiter
    - Löst CORS-Probleme
    - Vereinheitlicht API-Zugriffe

  * angular.json anpassen:
    ```json
    "options": {
      "buildTarget": "airbnb-clone-angular-frontend:build",
      "proxyConfig": "proxy.conf.mjs"
    }
    ```
    - Aktiviert Proxy-Konfiguration
    - Entwicklungsserver nutzt Proxy-Regeln

  * Zweck:
    - Nahtlose Frontend-Backend-Kommunikation
    - Entwicklungsumgebung spiegelt Produktion
    - Vereinfachte API-Aufrufe ohne absolute URLs


2. Auth0 Integration Testen
   * Startup-Verhalten:
     - CORS-Warnungen normal beim ersten Load
     - 302-Redirect Teil des Auth-Flows
     - Auth0-Login-Maske erscheint
   * Funktionen:
     - Login mit existierendem Account
     - Neue User-Registrierung
     - Google OAuth Option


📱 **FRONTEND IMPLEMENTATION**
3. Landlord Feature Implementierung
  * Model-Struktur:
    ```typescript
    /landlord/model/
    - listing-vo.model.ts (Value Objects)
    - listing.model.ts (Domain Models)
    - picture.model.ts (Bild-Handling)
    ```

  * Properties Create Flow:
    - Step-basierte Wizard-Komponente
    - Validierung pro Schritt
    - State Management für Listing-Erstellung
    - Schritte:
      1. Kategorie
      2. Location
      3. Infos
      4. Fotos
      5. Beschreibung
      6. Preis

  * Features:
    - Reaktive Formular-Updates
    - Progress Tracking
    - Error Handling
    - Toast Notifications
    - Navigation Guard

  * Architektur-Highlights:
    - Domain-Driven Design im Frontend
    - Value Objects für Typ-Sicherheit
    - Komponenten-basierte Struktur
    - Dependency Injection
    - Effect-basiertes State Management

  * Zweck:
    - Strukturierte Listing-Erstellung
    - User Experience Optimierung
    - Typ-sichere Datenmodellierung
    - Backend-Integration vorbereiten


📱 **FRONTEND IMPLEMENTATION**
4. Wizard Step-Komponenten
  * Category Step Implementation
    ```typescript
    @Component({
      selector: 'app-category-step',
      standalone: true
    })
    ```
    
  * Features:
    - Standalone Angular Component
    - Input/Output Binding
    - Dependency Injection (CategoryService)
    - Dynamic Category Selection
    - Validierung

  * Template-Struktur:
    ```html
    <div class="categories">
      - Überschrift
      - Kategorien-Grid
      - Interaktive Karten
      - Visuelles Feedback
    ```

  * Parent-Child Kommunikation:
    - Input: categoryName
    - Outputs:
      • categoryChange
      • stepValidityChange

  * Styling:
    - Flexbox Layout
    - Responsive Grid
    - Hover Effekte
    - Selektions-Feedback

  * Integration:
    - Switch-Case in Parent
    - Erste Wizard-Stufe
    - Kategorie-Auswahl
    - Validierungs-Flow

  * Zweck:
    - Benutzerfreundliche Kategorie-Auswahl
    - Klare visuelle Hierarchie
    - Reaktives Feedback
    - Erste Stufe des Listing-Wizards


📱 **FRONTEND IMPLEMENTATION**
5. Shared Footer Component für Wizard
  * Komponenten-Definition
    ```typescript
    @Component({
      selector: 'app-footer-step',
      standalone: true
    })
    ```

  * Core Features:
    - Step Navigation Controls
    - State Management:
      • Current Step Tracking
      • Validation Status
      • Loading States
    - Conditional Rendering
    - Event Handling

  * Template-Struktur:
    ```html
    <div class="controls">
      - Back Button (conditional)
      - Next/Finish Button
      - Loading Indicator
    ```

  * Styling:
    ```scss
    .controls {
      position: fixed;
      bottom: 0;
      width: 100%;
      height: 70px;
    }
    ```
    - Fixed Footer Layout
    - Responsive Design
    - Dialog Integration
    - Visual Feedback

  * Input/Output Pattern:
    - Inputs:
      • currentStep: Step
      • loading: boolean
      • isAllStepsValid: boolean
      • labelFinishedBtn: string
    - Outputs:
      • finish: EventEmitter
      • previous: EventEmitter
      • next: EventEmitter

  * Zweck:
    - Einheitliche Navigation im Wizard
    - Konsistente User Experience
    - Wiederverwendbare Komponente
    - Zentrales Step Management


📱 **FRONTEND IMPLEMENTATION**
6. Location Step Setup
  * Data Modeling
    ```typescript
    export interface Country {
      name: CountryName
      currencies: Currency
      capital: string[]
      region: string
      languages: { [key: string]: string }
      flag: string
    }
    ```
    - Typed Interfaces für Länder
    - Mehrsprachigkeit
    - Währungen und Regionen
    - Geographic Data

  * CountryService Implementation
    ```typescript
    @Injectable({
      providedIn: 'root'
    })
    ```
    - Signal basiertes State Management
    - HTTP Client Integration
    - RxJS Observables
    - Error Handling mit State Pattern
    - Caching via shareReplay
    - Länder-Lookup Funktionalität

  * Service Features:
    - Initialisierung mit JSON Daten
    - Ländersuche nach Code
    - Reactive State Management
    - Error Recovery
    - Performance Optimierung

  * Wizard Integration
    ```html
    <app-footer-step
      [currentStep]="currentStep"
      [isAllStepsValid]="isAllStepsValid()"
      (next)="nextStep()"
      (previous)="previousStep()"
    ></app-footer-step>
    ```
    - Navigation Flow
    - State Handling
    - Step Validierung
    - Loading States

  * Zweck:
    - Strukturierte Länderauswahl
    - Type-Safety
    - Reactive Architecture
    - Effizientes Daten-Management


🗺️ **FRONTEND IMPLEMENTATION**
7. Map Integration Setup
  * Dependencies:
    - @asymmetrik/ngx-leaflet: Angular Wrapper für Leaflet
    - leaflet: Open-Source Kartenbibliothek
      • Interaktive Karten
      • Marker
      • Layer Management
    - leaflet-geosearch: Geocoding-Funktionalität
      • Adresssuche
      • Koordinaten-Lookup
    - @types/leaflet: TypeScript Definitionen

  * Anwendungsfälle:
    - Standortauswahl für Listings
    - Adressvalidierung
    - Visuelle Standortdarstellung
    - Interaktive Location-Suche

  * Features:
    - Kostenlose Alternative zu Google Maps
    - OpenStreetMap Integration
    - Responsive Design
    - Custom Marker
    - Address Autocomplete


🗺️ **FRONTEND IMPLEMENTATION**
8. Location Map Implementation

  A. Location Map Component
  * Core Setup:
    ```typescript
    @Component({
      imports: [
        LeafletModule,
        FormsModule,
        AutoCompleteModule
      ]
    })
    ```
    - Leaflet Integration
    - Autocomplete Funktionalität
    - Form Handling

  * Features:
    - Interaktive Karte:
      • OpenStreetMap Integration
      • Custom Layer Control
      • Marker Management
    - Länderauswahl:
      • Autocomplete
      • Flaggen-Integration
      • Land-Koordinaten Mapping
    - State Management:
      • Location Tracking
      • Map Updates
      • Error Handling

  * Map Configuration:
    ```typescript
    options = {
      layers: [tileLayer("...")],
      zoom: 5,
      center: latLng(46.87996, -121.726909)
    }
    ```
    - Base Layers
    - Zoom Controls
    - Default Position

  B. Location Step Component
  * Structure:
    - Location Input/Output
    - Step Validation
    - Event Handling

  * Integration:
    - Parent-Child Communication
    - Step Flow Control
    - Location Updates

  * Features:
    - Location Selection Validation
    - Step Completion Logic
    - Event Propagation

  * Zweck:
    - Visuelle Standortauswahl
    - User-freundliche Ländersuche
    - Präzise Adressvalidierung
    - Nahtlose Wizard-Integration

  * Technical Highlights:
    - Leaflet Map Integration
    - OpenStreetMap Provider
    - Reactive Forms
    - Custom Event Handling
    - Toast Notifications
    - Effect-based State Management


📱 **FRONTEND IMPLEMENTATION**
9. Location UI Styling & Integration
  * Map Component Styling:
    ```scss
    ::ng-deep .map-location {
      height: calc(45vh);
      // Leaflet Controls Styling
    }
    ```
    - Custom Control Styling
    - Responsive Height
    - Font Integration
    - Search Bar Design

  * Angular Configuration:
    ```json
    "styles": [
      "src/styles.scss",
      "./node_modules/leaflet/dist/leaflet.css",
      "./node_modules/leaflet-geosearch/dist/geosearch.css"
    ]
    ```
    - Leaflet CSS Integration
    - Geosearch Styling
    - Global Style Imports

  * Location Step Template:
    ```html
    <div class="mb-3">
      <h1>Is the pin right in the spot?</h1>
      <h2>Your address is only shared...</h2>
      <app-location-map ...></app-location-map>
    </div>
    ```
    - User Instructions
    - Privacy Notice
    - Map Component Integration
    - Event Binding

  * Features:
    - Custom Styling
    - Responsive Design
    - Privacy-focused UI
    - Component Communication

  * Zweck:
    - Benutzerfreundliche Oberfläche
    - Klare Benutzerführung
    - Professionelles Styling
    - Framework Integration

📱 **FRONTEND IMPLEMENTATION**
10. Info Step Control Implementation
   * Core Component:
      ```typescript
      @Component({
        selector: 'app-info-step-control',
        standalone: true
      })
      ```

   * Features:
     - Numeric Control:
       ```typescript
       @Input() value: number
       @Input() minValue: number = 0
       @Output() valueChange = new EventEmitter<number>()
       ```
       • Increment/Decrement
       • Minimum Value Check
       • Value Emission

   * Template Structure:
     ```html
     <div class="info">
       - Title Display
       - Control Buttons
         • Decrement (with disable logic)
         • Current Value
         • Increment
       - Optional Separator
     ```

   * Styling:
     - Custom Button Design
     - Icons Integration
     - Responsive Layout
     - Visual Feedback:
       • Disabled States
       • Hover Effects
       • Spacing

   * Component Communication:
     - Input Properties:
       • title: string
       • value: number
       • minValue: number
       • separator: boolean
     - Output:
       • valueChange: number

   * Zweck:
     - Wiederverwendbare Steuerung
     - Numerische Eingabe
     - Validierung
     - Visuelles Feedback


📱 **FRONTEND IMPLEMENTATION**
11. Info Step Component Implementation
   * Komponenten-Definition:
     ```typescript
     @Component({
       selector: 'app-info-step',
       standalone: true
     })
     ```

   * Features:
     - Typ-Sicherheit durch Control Type:
       ```typescript
       export type Control = "GUESTS" | "BEDROOMS" | "BEDS" | "BATHS"
       ```
     - Signal-basierter Input:
       ```typescript
       infos = input.required();
       ```
     - Event Handling:
       ```typescript
       @Output() infoChange = new EventEmitter();
       @Output() stepValidityChange = new EventEmitter();
       ```

   * Validierung:
     - Minimum 1 Gast erforderlich
     - Echtzeit-Validierung
     - Event-basierte Updates

   * Switch-Case Pattern:
     ```typescript
     switch (valueType) {
       case "BATHS":
         this.infos().baths = {value: newValue}
         break;
       // ...weitere Cases
     }
     ```

   * Zweck:
     - Unterkunftsdetails erfassen
     - Validierung der Eingaben
     - Parent-Child Kommunikation
     - Typ-sichere Implementation


📱 **FRONTEND IMPLEMENTATION**
12. Info Step Integration in Properties Create
  * Template Integration (properties-create.component.html):
    ```html
    @case (INFO) {
      <div class="mb-5">
        <h1>Share some basics about your place</h1>
        <app-info-step
          [infos]="newListing.infos"
          (infoChange)="onInfoChange($event)"
          (stepValidityChange)="onValidityChange($event)"
        ></app-info-step>
      </div>
    }
    ```

  * Komponenten-Logik (properties-create.component.ts):
    ```typescript
    onInfoChange(newInfo: NewListingInfo) {
      this.newListing.infos = newInfo;
    }
    ```

  * Features:
    - Case-basierte Step-Anzeige
    - Zwei-Wege Datenbindung
    - Event Handling für:
      • Info Änderungen
      • Validitäts-Updates
    - Hierarchische Komponenten-Struktur

  * Integration:
    - Info Step als Child-Komponente
    - Parent verwaltet Listing-State
    - Validierungs-Flow von Child zu Parent

  * Zweck:
    - Nahtlose Wizard-Integration
    - State Management
    - Strukturierte Datenerfassung
    - Validierungs-Pipeline


📱 **FRONTEND IMPLEMENTATION**
13. Picture Step Implementation
  * Komponenten-Definition:
    ```typescript
    @Component({
      selector: 'app-picture-step',
      standalone: true,
      imports: [FontAwesomeModule, InputTextModule, ButtonModule]
    })
    ```

  * Core Features:
    - Signal-basierter Input für Bilder:
      ```typescript
      pictures = input.required<Array<NewListingPicture>>();
      ```
    - Event Handling:
      ```typescript
      @Output() picturesChange = new EventEmitter<Array<NewListingPicture>>();
      @Output() stepValidityChange = new EventEmitter<boolean>();
      ```
    - File Upload Logik:
      - Extraktion von Files
      - Validierung (min. 5 Bilder)
      - URL.createObjectURL für Preview
      - Löschfunktionalität

  * Template Features:
    - File Input mit Mehrfachauswahl
    - Bildergalerie mit Preview
    - Lösch-Button pro Bild
    - Responsive Grid Layout
    - Background-Image Display

  * Styling:
    ```scss
    .trash-btn {
      left: calc(100% - 70px);
      top: 10px;
    }
    .container-picture {
      width: 50%;
    }
    ```
    - Absolut positionierter Lösch-Button
    - 2-spaltige Bildergalerie
    - Responsive Container

  * Validierung:
    - Mindestens 5 Bilder erforderlich
    - Dateiformat-Überprüfung (.jpg,.jpeg,.png,.svg)
    - Echtzeit-Validierungs-Updates

  * Zweck:
    - Benutzerfreundlicher Bild-Upload
    - Sofortige Bildvorschau
    - Bilderverwaltung
    - Validierte Bildsammlung

**Zusätzliche Erklärungen:**
- Die Komponente nutzt URL.createObjectURL für effiziente Bildvorschauen
- Der File-Input akzeptiert nur bestimmte Bildformate
- Die Validierung erfolgt in Echtzeit bei Upload und Löschung
- Das Layout ist für Desktop optimiert (50% Bildbreite)
- Die Trash-Button Position ist präzise berechnet für optimale UX


📱 **FRONTEND IMPLEMENTATION**
14. Picture Step Integration in Properties Create
  * Template Integration:
    ```html
    @case (PHOTOS) {
      <app-picture-step
        [pictures]="newListing.pictures"
        (picturesChange)="onPictureChange($event)"
        (stepValidityChange)="onValidityChange($event)"
      ></app-picture-step>
    }
    ```

  * Features:
    - Case-basierte Integration (PHOTOS Step)
    - Bidirektionale Datenbindung:
      • Input: [pictures]
      • Output: (picturesChange)
    - Validierungs-Pipeline durch stepValidityChange
    - Nahtlose Wizard-Integration

  * Architektur:
    - Picture Step als eigenständige Komponente
    - Einbindung in Wizard-Flow
    - State Management über Parent
    - Event-basierte Kommunikation

  * Zweck:
    - Modulare Step-Integration
    - Zentrales State Management
    - Konsistente Validierung
    - Fluid User Experience

**Zusätzliche Erklärungen:**
- Der Case-Switch ermöglicht eine saubere Navigation
- Die Validierung wird an den Parent weitergeleitet
- Der Picture Step fügt sich nahtlos in den Wizard-Flow ein
- Die Komponenten-Hierarchie bleibt übersichtlich und wartbar


📱 **FRONTEND IMPLEMENTATION**
15. Description Step Implementation
  * Komponenten-Definition:
    ```typescript
    @Component({
      selector: 'app-description-step',
      standalone: true,
      imports: [InputTextModule, FormsModule, InputTextareaModule]
    })
    ```

  * Core Features:
    - Template-driven Forms:
      ```typescript
      @ViewChild("formDescription")
      formDescription: NgForm | undefined;
      ```
    - Signal-basierter Input:
      ```typescript
      description = input.required<Description>();
      ```
    - Event Handling:
      ```typescript
      @Output() descriptionChange = new EventEmitter<Description>();
      @Output() stepValidityChange = new EventEmitter<boolean>();
      ```

  * Template Features:
    - Responsive Form Layout
    - Titel Input (pInputText)
    - Mehrzeiliger Description Input (pInputTextarea)
    - Validierungsmeldungen
    - Placeholder für Validierung (21px Höhe)

  * Validierung:
    - Required Fields Check
    - Dirty-State Tracking
    - Live Validierungs-Updates
    - Form-Level Validierung

  * Event Handler:
    ```typescript
    onTitleChange(newTitle: string)
    onDescriptionChange(newDescription: string)
    validateForm(): boolean
    ```

  * Zweck:
    - Benutzerfreundliche Titeleingabe
    - Detaillierte Beschreibungsmöglichkeit
    - Sofortige Validierung
    - Konsistente Formularhöhe

**Zusätzliche Erklärungen:**
- Die Komponente nutzt PrimeNG Input-Komponenten
- Template-driven Forms für einfache Validierung
- Placeholder-div verhindert Layout-Sprünge
- Die Validierung erfolgt in Echtzeit


📱 **FRONTEND IMPLEMENTATION**
16. Description Step Integration in Properties Create
  * Template Integration:
    ```html
    @case (DESCRIPTION) {
      <app-description-step
        [description]="newListing.description"
        (descriptionChange)="onDescriptionChange($event)"
        (stepValidityChange)="onValidityChange($event)"
      ></app-description-step>
    }
    ```

  * Komponenten-Logik:
    ```typescript
    onDescriptionChange(newDescription: Description) {
      this.newListing.description = newDescription;
    }
    ```

  * Features:
    - Case-basierte Step-Integration
    - Bidirektionale Datenbindung
    - Validierungs-Pipeline
    - Zentrales State Management

  * Zweck:
    - Integration in Wizard-Flow
    - State Aktualisierung
    - Konsistente Datenverarbeitung
    - Nahtlose Benutzererfahrung

**Zusätzliche Erklärungen:**
- Der Description Step fügt sich nahtlos in den Wizard ein
- Die State-Verwaltung erfolgt zentral in der Parent-Komponente
- Die Validierung wird durchgängig berücksichtigt
- Das Pattern bleibt konsistent mit anderen Steps


📱 **FRONTEND IMPLEMENTATION**
17. Price Step Implementation
  * Komponenten-Definition:
    ```typescript
    @Component({
      selector: 'app-price-step',
      standalone: true,
      imports: [FormsModule, InputTextModule, FontAwesomeModule]
    })
    ```

  * Core Features:
    - Template-driven Form:
      ```typescript
      @ViewChild("formPrice") formPrice: NgForm
      ```
    - Preis Value Object:
      ```typescript
      price = input.required<PriceVO>();
      ```
    - Validierung:
      - Required Check
      - Minimum Wert (1)
      - Custom Error Messages

  * Template Highlights:
    - Dollar Icon Positionierung
    - Numerische Eingabe
    - Validierungsmeldungen
    - Placeholder System (21px)

  * Styling:
    ```scss
    .dollar {
      transform: translate(18px, 19px);
    }
    ```

📱 **FRONTEND IMPLEMENTATION**
18. Price Step Integration
  * Template Integration:
    ```html
    @case (PRICE) {
      <app-price-step
        [price]="newListing.price"
        (priceChange)="onPriceChange($event)"
        (stepValidityChange)="onValidityChange($event)"
      ></app-price-step>
    }
    ```

  * State Management:
    ```typescript
    onPriceChange(newPrice: PriceVO) {
      this.newListing.price = newPrice;
    }
    ```

  * Features beider Implementierungen:
    - Numerische Validierung
    - Echtzeit-Updates
    - Formatierte Preisdarstellung
    - Konsistente Fehlerbehandlung
    - Responsive Layout

**Zusätzliche Erklärungen:**
- Das Price Step System nutzt Value Objects für Typ-Sicherheit
- Die Integration folgt dem etablierten Wizard-Pattern
- Dollar-Icon ist präzise positioniert für optimale UX
- Validierung verhindert negative oder fehlende Preise


🔄 **SERVICE**
19. LandlordListingService Erweiterung

### A. Model Erweiterung
  * Neue Interfaces:
    ```typescript
    export interface DisplayPicture {
      file?: string,
      fileContentType?: string,
      isCover?: boolean
    }

    export interface CardListing {
      price: PriceVO,
      location: string,
      cover: DisplayPicture,
      bookingCategory: CategoryName,
      publicId: string,
      loading: boolean
    }
    ```

### B. Service Implementation
  * Neue Signals:
    ```typescript
    private getAll$ = signal(State.Builder<Array<CardListing>>().forInit())
    private delete$ = signal(State.Builder<string>().forInit())
    ```

  * CRUD Operationen:
    ```typescript
    // Read
    getAll(): void {
      this.http.get<Array<CardListing>>(`${environment.API_URL}/landlord-listing/get-all`)
    }

    // Delete
    delete(publicId: string): void {
      const params = new HttpParams().set("publicId", publicId);
      this.http.delete<string>(`${environment.API_URL}/landlord-listing/delete`, {params})
    }
    ```

  * State Management:
    - Signal-basiertes State Handling
    - Computed Properties für View Layer
    - Error Handling pro Operation
    - Reset Funktionalität

  * Features:
    - HTTP Params Handling
    - Type-safe Responses
    - Reactive State Updates
    - Clean Error States

**Zusätzliche Erklärungen:**
- Signals ermöglichen feingranulares State Management
- Builder Pattern für konsistente State Creation
- HTTP Params für sichere URL Parameter
- Reset Funktionen für State Cleanup


📦 **MODEL LAYER (Tenant Domain)**
20. Booking Model Implementation
  * Neue Interfaces:
    ```typescript
    export interface BookedDatesDTOFromServer {
      startDate: Date;
      endDate: Date;
    }

    export interface BookedListing {
      location: string,
      cover: DisplayPicture,
      totalPrice: PriceVO,
      dates: BookedDatesDTOFromServer,
      bookingPublicId: string,
      listingPublicId: string,
      loading: boolean
    }
    ```

📱 **SHARED COMPONENTS**
21. Card Listing Component Implementation
  * Komponenten-Definition:
    ```typescript
    @Component({
      selector: 'app-card-listing',
      standalone: true,
      imports: [DatePipe, CurrencyPipe, FaIconComponent]
    })
    ```

  * Core Features:
    - Dual Mode Support (landlord/booking)
    - Signal-basierte Inputs:
      ```typescript
      listing = input.required<CardListing | BookedListing>();
      cardMode = input<"landlord" | "booking">();
      ```
    - Event Handling:
      ```typescript
      @Output() deleteListing = new EventEmitter<CardListing>();
      @Output() cancelBooking = new EventEmitter<BookedListing>();
      ```

  * Effect Handlers:
    - Location Resolution via CountryService
    - Card Mode Switching
    - Dynamic Data Display

  * Template Features:
    - Responsive Card Layout
    - Dynamic Image Display
    - Conditional Action Buttons
    - Loading States
    - Mode-basierte Darstellung
    - Currency & Date Formatting

  * Routing:
    ```typescript
    onClickCard(publicId: string) {
      this.router.navigate(['listing'], {
        queryParams: {id: publicId}
      });
    }
    ```

  * Zweck:
    - Wiederverwendbare Listing-Karte
    - Dual-Use für Vermieter und Mieter
    - Einheitliches Design
    - Responsive Benutzerinteraktion


📱 **FRONTEND IMPLEMENTATION**
22. Properties Component & Routing Implementation

### A. Properties Component
  * Komponenten-Definition:
    ```typescript
    @Component({
      selector: 'app-properties',
      standalone: true,
      imports: [CardListingComponent, FaIconComponent]
    })
    ```

  * State Management:
    - Listings Array
    - Loading States (Deletion & Fetch)
    - Effect-basierte State Updates

  * Core Features:
    - Listing Verwaltung:
      ```typescript
      private fetchListings()
      onDeleteListing(listing: CardListing)
      ```
    - State Listener:
      ```typescript
      private listenFetchAll()
      private listenDeleteByPublicId()
      ```
    - Toast Notifications
    - Loading Indicators

  * Template Features:
    - Responsive Grid Layout
    - Empty State Handling
    - Loading Animations
    - Conditional Rendering

### B. Route Configuration
  * Routes Definition:
    ```typescript
    export const routes: Routes = [
      {
        path: 'landlord/properties',
        component: PropertiesComponent,
        canActivate: [authorityRouteAccess],
        data: {
          authorities: ["ROLE_LANDLORD"]
        }
      }
    ];
    ```

### C. Authority Route Guard
  * Guard Implementation:
    ```typescript
    export const authorityRouteAccess: CanActivateFn = 
      (next: ActivatedRouteSnapshot, state: RouterStateSnapshot) => {
        // Authentifizierung & Autorisierung
    }
    ```

  * Features:
    - Role-based Access Control
    - Automatic Login Redirect
    - Authority Verification
    - RxJS Integration

  * Zweck:
    - Geschützte Routes
    - Authority-basierte Navigation
    - Sichere Komponenten-Zugriffe
    - User Experience Management


📱 **FRONTEND IMPLEMENTATION**
23. Category Component Navigation Enhancement

### A. Router Integration
  * Neue Properties:
    ```typescript
    isHome = false;
    router = inject(Router);
    activatedRoute = inject(ActivatedRoute);
    ```

### B. Navigation Logik
  * Router Event Handling:
    ```typescript
    private listenRouter() {
      this.router.events.pipe(
        filter((evt): evt is NavigationEnd => evt instanceof NavigationEnd)
      ).subscribe({
        next: (evt: NavigationEnd) => {
          this.isHome = evt.url.split("?")[0] === "/";
          // Home Route Default Category Logic
        }
      });
    }
    ```

  * Query Parameter Handling:
    ```typescript
    this.activatedRoute.queryParams
      .pipe(
        map(params => params["category"])
      )
      .subscribe({
        next: (categoryName: CategoryName) => {
          // Category Activation Logic
        }
      })
    ```

### C. Category Management
  * Neue Methoden:
    ```typescript
    private activateCategory(category: Category) {
      // Category State Management
    }

    onChangeCategory(category: Category) {
      // Navigation mit Query Params
    }
    ```

  * Features:
    - Home Page Detection
    - Default Category (ALL)
    - Query Parameter Based Navigation
    - Category State Management
    - Router Event Filtering
    - Active Category Tracking

  * Zweck:
    - URL-basierte Kategoriefilterung
    - Saubere Navigation
    - State Management
    - Home Page Spezialbehandlung


📦 **MODEL LAYER**
24. Request Model Implementation
  * Core Interfaces:
    ```typescript
    export interface Pagination {
      page: number;
      size: number;
      sort: string[];
    }

    export interface Page<T> {
      content: T[];
      pageable: Pageable;
      // ... weitere Page Metadaten
    }
    ```
  * Helper Function:
    ```typescript
    export const createPaginationOption = 
      (req: Pagination): HttpParams
    ```

🔄 **SERVICE LAYER**
25. TenantListingService Implementation
  * Service Definition:
    ```typescript
    @Injectable({ providedIn: 'root' })
    export class TenantListingService
    ```

  * State Management:
    ```typescript
    private getAllByCategory$ = signal(
      State.Builder<Page<CardListing>>().forInit()
    )
    ```

  * Core Features:
    - Paginierte Kategorie-Abfrage
    - State Reset
    - Error Handling
    - Parameter Building

## 📱 COMPONENT LAYER
26. Home Component Implementation
  * Component Setup:
    ```typescript
    @Component({
      selector: 'app-home',
      standalone: true
    })
    ```

  * Core Features:
    - Category Change Listener
    - Pagination Handling
    - Loading States
    - Error Management
    - State Reset
    - Effect-basiertes State Management

  * Template Features:
    - Loading Indicator
    - Grid Layout
    - Empty States
    - Conditional Rendering

  * Zweck:
    - Homepage Implementation
    - Kategoriebasierte Listing-Anzeige
    - Responsive Layout
    - User Feedback

# Frontend Tutorial Notes

## 📦 MODEL LAYER
27. Listing Model Erweiterung
  * Neue Interfaces:
    ```typescript
    export interface Listing {
      description: Description,
      pictures: Array<DisplayPicture>,
      infos: NewListingInfo,
      price: PriceVO,
      category: CategoryName,
      location: string,
      landlord: LandlordListing
    }

    export interface LandlordListing {
      firstname: string,
      imageUrl: string,
    }
    ```


🔄 **SERVICE LAYER**
28. TenantListingService Erweiterung
  * Neue State Management Features:
    ```typescript
    private getOneByPublicId$ = signal(
      State.Builder<Listing>().forInit()
    )
    getOneByPublicIdSig = computed(() => 
      this.getOneByPublicId$()
    )
    ```

  * API Integration:
    ```typescript
    getOneByPublicId(publicId: string): void {
      const params = new HttpParams()
        .set("publicId", publicId);
      this.http.get<Listing>(
        `${environment.API_URL}/tenant-listing/get-one`, 
        {params}
      )
    }
    ```

  * State Management:
    - Signal-basiertes State Handling
    - Computed Properties
    - State Reset Funktionalität
    - Error Handling

  * Features:
    - Einzelnes Listing abrufen
    - Query Parameter Handling
    - Type-safe Responses
    - State Lifecycle Management

**Zweck:**
- Detailansicht vorbereiten
- Typ-sicheres State Management
- Clean Service Architecture
- Error Handling Integration


📱 **FRONTEND IMPLEMENTATION**
29. Display Listing Component Implementation

### A. Component Logic
  * Core Setup:
    ```typescript
    @Component({
      selector: 'app-display-listing',
      standalone: true,
      imports: [NgClass, FaIconComponent, AvatarComponent]
    })
    ```

  * State Management:
    - Service Injections
    - Loading States
    - Router Parameter Extraktion
    - Effect-basiertes State Management

  * Core Features:
    ```typescript
    private listenToFetchListing() {
      effect(() => {
        // State Management & Data Transformation
        // Country Resolution
        // Cover Picture Handling
      });
    }
    ```

  * Helper Functions:
    ```typescript
    private putCoverPictureFirst(pictures: Array<DisplayPicture>)
    private extractIdParamFromRouter()
    private fetchListing(publicId: string)
    ```

### B. Template Implementation
  * Layout Struktur:
    - Bildergalerie
    - Listing Details
    - Host Information
    - Beschreibung

  * Features:
    - Responsive Galerie Layout
    - Conditional Rendering
    - Loading States
    - Icon Integration
    - Dynamic Styling:
      ```html
      [style.background-image]="'url(' + 'data:' + 
        picture.fileContentType + ';base64,' + picture.file + ')'"
      ```

  * Zweck:
    - Detaillierte Listing-Ansicht
    - Rich Media Display
    - User Information
    - Property Details
    - Loading Feedback

**Key Features:**
- Bildergalerie mit Cover First
- Responsive Layout
- Detaillierte Property Informationen
- Host Details Integration
- Loading States


🎨 **STYLING IMPLEMENTATION**
30. Display Listing Gallery & List Styling

### A. Grid Gallery Layout
  * Grid Setup:
    ```scss
    .gallery {
      display: grid;
      grid-template-columns: repeat(8, 1fr);
      grid-template-rows: repeat(6, 5vw);
      grid-gap: 10px;
    }
    ```

  * Picture Positioning:
    ```scss 
    .cover {
      grid-column-start: 1;
      grid-column-end: 5;
      grid-row-start: 1;
      grid-row-end: 7;
      // Rounded corners
    }
    ```

  * Features:
    - 8x6 Grid System
    - Responsive Sizing (vw units)
    - Asymmetrisches Layout
    - Abgerundete Ecken
    - Definierte Bildpositionen

### B. List Styling
  * List Reset & Customization:
    ```scss
    ol {
      list-style-type: circle;
      margin-block-start: 0.3em;
      // ... weitere Resets
    }
    ```

  * List Items:
    ```scss
    li {
      display: inline-block;
      margin-right: 6px;
    }
    ```

  * Icon Styling:
    ```scss
    fa-icon {
      font-size: 3px;
      vertical-align: middle;
      margin-right: 4px;
    }
    ```

  * Features:
    - Inline List Items
    - Custom Bullet Points
    - Icon Integration
    - Feine Spacing Kontrolle
    - Reset Browser Defaults

**Zweck:**
- Professionelles Bildergalerie-Layout
- Responsive Design
- Konsistente Listendarstellung
- Clean Visual Hierarchy


🛣️ **ROUTING IMPLEMENTATION**
31. Display Listing Route Integration
  * Route Configuration:
    ```typescript
    export const routes: Routes = [
      {
        path: 'landlord/properties',
        component: PropertiesComponent,
        canActivate: [authorityRouteAccess],
        data: {
          authorities: ["ROLE_LANDLORD"]
        }
      },
      {
        path: '',
        component: HomeComponent
      },
      {
        path: 'listing',
        component: DisplayListingComponent
      }
    ];
    ```

  * Features:
    - Public Route (kein Guard)
    - Query Parameter basiert (?id=xyz)
    - Direkter Component Mount
    - Integriert mit Home/Properties Routes

  * Routing Flow:
    - Home → Listing Card Click → Display Listing
    - Properties → Listing Card Click → Display Listing
    - URL Pattern: `/listing?id=[publicId]`

  * Zweck:
    - Detailansicht Routing
    - Öffentlicher Zugriff
    - Clean URLs
    - Seamless Navigation


📱 **FRONTEND IMPLEMENTATION**
32. Date Management Setup
  * Package Installation:
    ```bash
    npm install dayjs
    ```
  * Zweck:
    - Effizientes Datum-Handling
    - Zeitzonen-Management
    - Date Range Berechnungen

📦 **MODEL LAYER**
33. Booking Model Implementation
  * Neue Interfaces in booking.model.ts:
    ```typescript
    export interface CreateBooking {
      startDate: Date,
      endDate: Date,
      listingPublicId: string,
    }

    export interface BookedDatesDTOFromClient {
      startDate: Dayjs,
      endDate: Dayjs,
    }

    export interface BookedDatesDTOFromServer {
      startDate: Date,
      endDate: Date,
    }
    ```
  * Features:
    - Typsichere Buchungserstellung
    - Client/Server DTO Trennung
    - DayJS Integration

🔄 **SERVICE LAYER**
34. BookingService Implementation
  * Core Features:
    - Signals für State Management
    - HTTP Endpoints:
      • Buchung erstellen
      • Verfügbarkeit prüfen
      • Buchungen abrufen
      • Buchungen stornieren
    - Date Mapping Utilities
    - State Reset Funktionen

📱 **COMPONENT LAYER**
35. BookDate Component Implementation
  * Template Features:
    - PrimeNG Calendar Integration
    - Range Selection
    - Verfügbarkeitsprüfung
    - Preis Kalkulation
    - Auth Check
    - Responsive Layout

  * Komponenten-Logik:
    - Date Range Handling
    - Buchungsvalidierung
    - Auth Integration
    - Toast Notifications
    - Router Navigation

  * State Management:
    - Booking Dates Tracking
    - Total Price Calculation
    - Availability Checks
    - Booking Creation Flow

📱 **FRONTEND IMPLEMENTATION**
36. Display Listing Component Erweiterung
  * Integration BookDate Component:
    ```html
    <app-book-date 
      [listingPublicId]="currentPublicId" 
      [listing]="listing">
    </app-book-date>
    ```
    
  * Layout Features:
    - Zwei-Spalten Layout (Content & Booking)
    - Responsive Design
    - Gallery Grid bleibt links
    - Booking Widget rechts fixiert
    - Flexbox für Content-Bereich

  * Content Struktur:
    - Listing Title
    - Bildergalerie
    - Property Details mit Icons
    - Host Information
    - Beschreibung
    - Buchungsformular

  * Backend Anpassung:
    - POST `api/booking/check-availability` in SecurityConfiguration.java public gesetzt
    - Ermöglicht Buchungszugriff für authentifizierte User

  * Zweck:
    - Integration des Buchungssystems
    - Vollständige Listing-Detailansicht
    - User-freundliches Buchungsinterface
    - Nahtlose Backend-Kommunikation


📦 **MODEL LAYER**
37. Booking Model Erweiterung
 * BookedListing Interface:
   ```typescript
   export interface BookedListing {
     location: string,
     cover: DisplayPicture,
     totalPrice: PriceVO,
     dates: BookedDatesDTOFromServer,
     bookingPublicId: string,
     listingPublicId: string,
     loading: boolean
   }
   ```
 * Features:
   - Strukturierte Buchungsdetails
   - Integration mit bestehenden Types
   - Loading State für UI-Feedback

🔄 **SERVICE LAYER**
38. BookingService Erweiterung
 * Neue Signals:
   ```typescript
   private getBookedListing$ = signal(
     State.Builder<Array<BookedListing>>().forInit()
   );
   getBookedListingSig = computed(() => 
     this.getBookedListing$()
   );

   private cancel$ = signal(
     State.Builder<string>().forInit()
   );
   cancelSig = computed(() => this.cancel$());
   ```

 * Service Methoden:
   - getBookedListing(): Abruf aller Buchungen
   - cancel(): Stornierung einer Buchung
   - resetCancel(): State Reset nach Stornierung

 * HTTP Integration:
   - GET /booking/get-booked-listing
   - DELETE /booking/cancel mit Query Params
   - State-basiertes Error Handling
   - Type-safe Responses

 * Zweck:
   - Buchungsverwaltung
   - Stornierungsfunktionalität
   - Reaktives State Management
   - Error Handling


📱 **FRONTEND IMPLEMENTATION**
39. BookedListing Component Implementation
 * Component Setup:
   ```typescript
   @Component({
     selector: 'app-booked-listing',
     standalone: true,
     imports: [
       CardListingComponent,
       FaIconComponent
     ]
   })
   ```

 * Core Features:
   - Buchungsübersicht
   - Stornierungsfunktion
   - Loading States
   - Effect-basiertes State Management
   - Toast Notifications

 * Template Struktur:
   - Überschriften
   - Listing Grid mit CardListing Components
   - Empty State Handling
   - Loading Spinner
   - Responsive Layout

 * State Management:
   ```typescript
   private listenFetchBooking() {
     effect(() => {
       const bookedListingsState = this.bookingService.getBookedListingSig();
       // State handling & UI updates
     });
   }
   ```

 * Cancellation Flow:
   - Stornierungsanfrage
   - Loading State pro Listing
   - Success/Error Handling
   - Array Update nach Stornierung

🛣️ **ROUTING**
40. Booking Route Integration
 * Route Definition:
   ```typescript
   {
     path: "booking",
     component: BookedListingComponent
   }
   ```
 * Features:
   - Direkte Navigation zu Buchungen
   - Component Lazy Loading
   - Clean URL Structure


🔄 **SERVICE LAYER**
41. BookingService Landlord Erweiterung
 * Cancel Methode Update:
   ```typescript
   cancel(
     bookingPublicId: string, 
     listingPublicId: string, 
     byLandlord: boolean
   )
   ```

 * Neues Signal:
   ```typescript
   private getBookedListingForLandlord$ = signal(
     State.Builder<Array<BookedListing>>().forInit()
   );
   getBookedListingForLandlordSig = computed(() => 
     this.getBookedListingForLandlord$()
   );
   ```

 * Neue Service Methode:
   - getBookedListingForLandlord()
   - HTTP GET Integration
   - State Management
   - Error Handling

📱 **FRONTEND IMPLEMENTATION**
42. Reservation Component
 * Component Setup:
   ```typescript
   @Component({
     selector: 'app-reservation',
     standalone: true,
     imports: [
       CardListingComponent,
       FaIconComponent
     ]
   })
   ```

 * Core Features:
   - Reservierungsübersicht für Landlords
   - Stornierungsfunktion
   - Effect-basiertes State Management
   - Loading States
   - Toast Notifications

 * Template Structure:
   - Überschriften-Hierarchie
   - Responsive Grid Layout
   - Empty State Handling
   - Loading Animation
   - Card-basierte Darstellung

🛣️ **ROUTING**
43. Landlord Route Integration
 ```typescript
 {
   path: "landlord/reservation",
   component: ReservationComponent,
   canActivate: [authorityRouteAccess],
   data: {
     authorities: ["ROLE_LANDLORD"]
   }
 }
 ```

  * Features:
   - Geschützte Route
   - Rollenbasierte Zugriffskontrolle
   - Landlord-spezifischer Pfad
   - Authority Data Config


# 🔍 FRONTEND IMPLEMENTATION (Search Feature)
## 44. Implementierung der Such-Funktionalität

### A. Service Layer Erweiterung
* TenantListingService:
 * Search Signal Definition:
   * `private search$: Subject<State<Page<CardListing>>>`
   * `search = this.search$.asObservable()`
 * Search Methode:
   * `searchListing(newSearch: Search, pageRequest: Pagination)`
   * HTTP POST Integration mit Pagination
   * State-basiertes Error Handling

### B. Model Layer
* search.model.ts:
 * Interface Definition:
   * `Search` Interface mit:
     * location: string
     * dates: BookedDatesDTOFromServer  
     * infos: NewListingInfo
   * Integration mit bestehenden Models
   * Typ-sichere Suchparameter

### C. Component Layer
* SearchComponent:
 * Drei-Stufen Wizard:
   * Location Auswahl
   * Datums Selektion
   * Gäste/Räume Info
 * Features:
   * Step Management
   * State Handling
   * Router Navigation
   * Dynamic Dialog Integration
 * Methoden:
   * Step Navigation (next/previous)
   * Validierung
   * Event Handling für:
     * Location Updates
     * Date Changes
     * Info Updates
   * Search Execution mit Query Params

### D. Template Structure
* Conditional Step Rendering via @switch
* Integration von:
 * LocationMap Component
 * SearchDate Component
 * InfoStep Component
* Footer Navigation
* Responsive Layout

### E. Type Safety
* Step Model Definition
* Search Interface Implementation
* Strongly Typed Event Handling
* Date Format Standardization


# 🔍 FRONTEND IMPLEMENTATION (Search Feature)
## 45. Suchmodal und Datumsauswahl

### A. Navbar Integration
* Navbar Component Erweiterung:
 * `openNewSearch()` Methode:
   * Modal-Dialog via PrimeNG
   * Konfiguration:
     * 40% Breite
     * Modal mit Header
     * Schließbar
     * Focus Handling
 * Template Anpassung:
   * Such-Button Gruppe
   * Responsive Layout
   * Click Handler für Modal
   * Visuelle Trenner zwischen Elementen

### B. SearchDate Component
* Core Features:
 * Signal-basierter Input für Dates
 * Kalender Integration via PrimeNG
 * Two-Way Binding
 * Validierung
 * State Wiederherstellung

* Komponenten-Logik:
 * Date Range Validierung:
   * Start- und Enddatum erforderlich
   * Unterschiedliche Tage
   * Mindestdatum (heute)
 * Event Handling:
   * Date Change Events
   * Validity Change Events
 * Effect für Date Restoration

* Template Structure:
 * Inline Kalender
 * Range Selection Mode
 * Touch UI Support
 * Responsive Layout
 * Zentrierte Darstellung

### C. Styling & Layout
* Responsive Design:
 * Maximale Breite für Kalender
 * Zentrierte Ausrichtung
 * Touch-freundliche UI
* Visuelle Hierarchie:
 * Klare Trenner
 * Konsistente Abstände
 * Hover Effekte


# 🔍 FRONTEND IMPLEMENTATION (Search Integration)
## 46. Such-Integration in Home und Navbar

### A. Navbar Erweiterung
* Information Extraction:
 * `extractInformationForSearch()` Methode:
   * Query Parameter Auswertung
   * Formatierung von:
     * Location Display
     * Guest Count
     * Date Range
   * Fallback Werte
 * Integration in Component Lifecycle (ngOnInit)

### B. Home Component Integration
* Search Listener Implementation:
 * Status Management:
   * OK: Listings Update
   * ERROR: Toast Message
 * Loading States
 * Empty Results Handling

* Neue Such-Methoden:
 * `startNewSearch()`:
   * Query Parameter Verarbeitung
   * Search Object Aufbau
   * Service Integration
 * `onResetSearchFilter()`:
   * Filter Reset
   * Navigation zur Default Category
   * Loading State Reset

* Template Updates:
 * Conditional Rendering für:
   * Loading Spinner
   * Listing Grid
   * "No Results" States
   * Reset Option
 * Responsive Grid Layout
 * User Feedback

### C. Daten-Flow
* Query Parameters → Navbar Display
* Query Parameters → Search Execution
* Search Results → Listing Display
* Error States → Toast Notifications