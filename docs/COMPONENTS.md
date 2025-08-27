# Components Documentation

## Overview

This document provides comprehensive documentation for all UI components in the codebase, including their props, usage examples, and styling options.

## Table of Contents

- [Layout Components](#layout-components)
- [Form Components](#form-components)
- [Data Display Components](#data-display-components)
- [Navigation Components](#navigation-components)
- [Feedback Components](#feedback-components)
- [Utility Components](#utility-components)
- [Component Composition](#component-composition)
- [Styling and Theming](#styling-and-theming)

## Layout Components

### Container

A responsive container component that centers content and provides consistent spacing.

**Props:**
- `children: ReactNode` - Content to render inside container
- `maxWidth?: 'sm' | 'md' | 'lg' | 'xl' | 'full'` - Maximum width constraint
- `padding?: 'none' | 'sm' | 'md' | 'lg'` - Padding around content
- `centered?: boolean` - Whether to center content horizontally
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Container } from './components/Container';

<Container maxWidth="lg" padding="md" centered>
  <h1>Welcome to our app</h1>
  <p>This content is centered and has consistent spacing.</p>
</Container>
```

**Default Styling:**
```css
.container {
  width: 100%;
  margin: 0 auto;
  padding: 0 1rem;
}

.container--sm { max-width: 640px; }
.container--md { max-width: 768px; }
.container--lg { max-width: 1024px; }
.container--xl { max-width: 1280px; }
.container--full { max-width: none; }

.container--padding-sm { padding: 1rem; }
.container--padding-md { padding: 1.5rem; }
.container--padding-lg { padding: 2rem; }
```

### Grid

A flexible grid system for creating responsive layouts.

**Props:**
- `children: ReactNode` - Grid items
- `columns?: number` - Number of columns (default: 12)
- `gap?: 'none' | 'sm' | 'md' | 'lg'` - Gap between grid items
- `alignItems?: 'start' | 'center' | 'end' | 'stretch'` - Vertical alignment
- `justifyContent?: 'start' | 'center' | 'end' | 'space-between' | 'space-around'` - Horizontal alignment
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Grid, GridItem } from './components/Grid';

<Grid columns={3} gap="md" alignItems="center">
  <GridItem span={1}>
    <div>Left sidebar</div>
  </GridItem>
  <GridItem span={2}>
    <div>Main content</div>
  </GridItem>
</Grid>
```

**GridItem Props:**
- `children: ReactNode` - Content to render
- `span: number` - Number of columns to span
- `offset?: number` - Number of columns to offset
- `className?: string` - Additional CSS classes

### Flexbox

A flexible container for creating flexible layouts.

**Props:**
- `children: ReactNode` - Flex items
- `direction?: 'row' | 'row-reverse' | 'column' | 'column-reverse'` - Flex direction
- `wrap?: 'nowrap' | 'wrap' | 'wrap-reverse'` - Flex wrap behavior
- `justifyContent?: 'start' | 'center' | 'end' | 'space-between' | 'space-around' | 'space-evenly'` - Main axis alignment
- `alignItems?: 'start' | 'center' | 'end' | 'stretch' | 'baseline'` - Cross axis alignment
- `gap?: 'none' | 'sm' | 'md' | 'lg'` - Gap between items
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Flex } from './components/Flex';

<Flex direction="row" justifyContent="space-between" alignItems="center" gap="md">
  <div>Logo</div>
  <nav>Navigation</nav>
  <div>User menu</div>
</Flex>
```

## Form Components

### Input

A versatile input component with various types and validation states.

**Props:**
- `type?: 'text' | 'email' | 'password' | 'number' | 'tel' | 'url' | 'search'` - Input type
- `value: string` - Input value
- `onChange: (value: string) => void` - Change handler
- `placeholder?: string` - Placeholder text
- `label?: string` - Input label
- `error?: string` - Error message
- `disabled?: boolean` - Whether input is disabled
- `required?: boolean` - Whether input is required
- `size?: 'sm' | 'md' | 'lg'` - Input size
- `variant?: 'outlined' | 'filled' | 'underlined'` - Input style variant
- `icon?: ReactNode` - Icon to display
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Input } from './components/Input';
import { SearchIcon } from './icons';

const [searchValue, setSearchValue] = useState('');

<Input
  type="search"
  value={searchValue}
  onChange={setSearchValue}
  placeholder="Search..."
  label="Search"
  icon={<SearchIcon />}
  size="md"
  variant="outlined"
/>
```

**Validation Example:**
```jsx
const [email, setEmail] = useState('');
const [emailError, setEmailError] = useState('');

const validateEmail = (value) => {
  if (!value) {
    setEmailError('Email is required');
    return false;
  }
  if (!/\S+@\S+\.\S+/.test(value)) {
    setEmailError('Invalid email format');
    return false;
  }
  setEmailError('');
  return true;
};

<Input
  type="email"
  value={email}
  onChange={(value) => {
    setEmail(value);
    validateEmail(value);
  }}
  label="Email"
  error={emailError}
  required
/>
```

### Select

A dropdown select component with search and multi-select capabilities.

**Props:**
- `options: Array<{value: string, label: string, disabled?: boolean}>` - Available options
- `value: string | string[]` - Selected value(s)
- `onChange: (value: string | string[]) => void` - Change handler
- `placeholder?: string` - Placeholder text
- `label?: string` - Select label
- `multiple?: boolean` - Whether multiple selection is allowed
- `searchable?: boolean` - Whether options are searchable
- `disabled?: boolean` - Whether select is disabled
- `size?: 'sm' | 'md' | 'lg'` - Select size
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Select } from './components/Select';

const countries = [
  { value: 'us', label: 'United States' },
  { value: 'ca', label: 'Canada' },
  { value: 'uk', label: 'United Kingdom' },
  { value: 'de', label: 'Germany' }
];

const [selectedCountry, setSelectedCountry] = useState('');

<Select
  options={countries}
  value={selectedCountry}
  onChange={setSelectedCountry}
  placeholder="Select a country"
  label="Country"
  searchable
/>
```

**Multi-select Example:**
```jsx
const [selectedLanguages, setSelectedLanguages] = useState([]);

const languages = [
  { value: 'javascript', label: 'JavaScript' },
  { value: 'typescript', label: 'TypeScript' },
  { value: 'python', label: 'Python' },
  { value: 'java', label: 'Java' }
];

<Select
  options={languages}
  value={selectedLanguages}
  onChange={setSelectedLanguages}
  placeholder="Select languages"
  label="Programming Languages"
  multiple
  searchable
/>
```

### Checkbox

A checkbox component with various states and styling options.

**Props:**
- `checked: boolean` - Whether checkbox is checked
- `onChange: (checked: boolean) => void` - Change handler
- `label?: string` - Checkbox label
- `disabled?: boolean` - Whether checkbox is disabled
- `indeterminate?: boolean` - Whether checkbox is in indeterminate state
- `size?: 'sm' | 'md' | 'lg'` - Checkbox size
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Checkbox } from './components/Checkbox';

const [isChecked, setIsChecked] = useState(false);

<Checkbox
  checked={isChecked}
  onChange={setIsChecked}
  label="I agree to the terms and conditions"
  size="md"
/>
```

**Indeterminate State Example:**
```jsx
const [items, setItems] = useState([
  { id: 1, name: 'Item 1', checked: false },
  { id: 2, name: 'Item 2', checked: false },
  { id: 3, name: 'Item 3', checked: false }
]);

const allChecked = items.every(item => item.checked);
const someChecked = items.some(item => item.checked);

const handleSelectAll = (checked) => {
  setItems(items.map(item => ({ ...item, checked })));
};

<Checkbox
  checked={allChecked}
  indeterminate={someChecked && !allChecked}
  onChange={handleSelectAll}
  label="Select All"
/>
```

### RadioGroup

A group of radio buttons for single selection.

**Props:**
- `options: Array<{value: string, label: string, disabled?: boolean}>` - Available options
- `value: string` - Selected value
- `onChange: (value: string) => void` - Change handler
- `name: string` - Form field name
- `direction?: 'horizontal' | 'vertical'` - Layout direction
- `disabled?: boolean` - Whether all radios are disabled
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { RadioGroup } from './components/RadioGroup';

const [selectedPlan, setSelectedPlan] = useState('basic');

const plans = [
  { value: 'basic', label: 'Basic Plan' },
  { value: 'pro', label: 'Pro Plan' },
  { value: 'enterprise', label: 'Enterprise Plan' }
];

<RadioGroup
  options={plans}
  value={selectedPlan}
  onChange={setSelectedPlan}
  name="plan"
  direction="vertical"
/>
```

### Textarea

A multi-line text input component.

**Props:**
- `value: string` - Textarea value
- `onChange: (value: string) => void` - Change handler
- `placeholder?: string` - Placeholder text
- `label?: string` - Textarea label
- `rows?: number` - Number of visible rows
- `maxLength?: number` - Maximum character length
- `resizable?: boolean` - Whether textarea is resizable
- `error?: string` - Error message
- `disabled?: boolean` - Whether textarea is disabled
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Textarea } from './components/Textarea';

const [description, setDescription] = useState('');

<Textarea
  value={description}
  onChange={setDescription}
  placeholder="Enter description..."
  label="Description"
  rows={4}
  maxLength={500}
  resizable
/>
```

## Data Display Components

### Table

A comprehensive table component with sorting, filtering, and pagination.

**Props:**
- `data: Array<Record<string, any>>` - Table data
- `columns: Array<Column>` - Column definitions
- `sortable?: boolean` - Whether columns are sortable
- `searchable?: boolean` - Whether table has search functionality
- `pagination?: boolean` - Whether to show pagination
- `pageSize?: number` - Number of items per page
- `onRowClick?: (row: any, index: number) => void` - Row click handler
- `loading?: boolean` - Whether table is loading
- `emptyMessage?: string` - Message when no data
- `className?: string` - Additional CSS classes

**Column Definition:**
```typescript
interface Column {
  key: string;
  label: string;
  sortable?: boolean;
  width?: string | number;
  render?: (value: any, row: any) => ReactNode;
  align?: 'left' | 'center' | 'right';
}
```

**Example:**
```jsx
import { Table } from './components/Table';

const users = [
  { id: 1, name: 'John Doe', email: 'john@example.com', role: 'Admin' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com', role: 'User' }
];

const columns = [
  { key: 'name', label: 'Name', sortable: true },
  { key: 'email', label: 'Email', sortable: true },
  { 
    key: 'role', 
    label: 'Role',
    render: (value) => (
      <span className={`badge badge--${value.toLowerCase()}`}>
        {value}
      </span>
    )
  }
];

<Table
  data={users}
  columns={columns}
  sortable
  searchable
  pagination
  pageSize={10}
  onRowClick={(row) => console.log('Clicked row:', row)}
/>
```

### Card

A versatile card component for displaying content in containers.

**Props:**
- `children: ReactNode` - Card content
- `title?: string` - Card title
- `subtitle?: string` - Card subtitle
- `image?: string` - Card image URL
- `actions?: ReactNode` - Action buttons
- `elevation?: 'none' | 'sm' | 'md' | 'lg'` - Shadow elevation
- `padding?: 'none' | 'sm' | 'md' | 'lg'` - Content padding
- `clickable?: boolean` - Whether card is clickable
- `onClick?: () => void` - Click handler
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Card, Button } from './components';

<Card
  title="Product Name"
  subtitle="Product description goes here"
  image="/product-image.jpg"
  elevation="md"
  padding="md"
  actions={
    <div className="card-actions">
      <Button variant="primary" size="sm">Add to Cart</Button>
      <Button variant="secondary" size="sm">View Details</Button>
    </div>
  }
>
  <p>Additional product information can go here.</p>
</Card>
```

**Clickable Card Example:**
```jsx
<Card
  title="Clickable Card"
  clickable
  onClick={() => navigate('/details')}
  elevation="sm"
>
  <p>Click to view details</p>
</Card>
```

### Badge

A small component for displaying status, counts, or labels.

**Props:**
- `children: ReactNode` - Badge content
- `variant?: 'primary' | 'secondary' | 'success' | 'warning' | 'danger' | 'info'` - Badge style
- `size?: 'sm' | 'md' | 'lg'` - Badge size
- `rounded?: boolean` - Whether badge has rounded corners
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Badge } from './components/Badge';

<Badge variant="success" size="md">Active</Badge>
<Badge variant="warning" size="sm">Pending</Badge>
<Badge variant="danger" size="lg">Error</Badge>
<Badge variant="info" rounded>New</Badge>
```

### Avatar

A component for displaying user profile pictures or initials.

**Props:**
- `src?: string` - Image source URL
- `alt?: string` - Image alt text
- `name?: string` - User name for initials
- `size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl'` - Avatar size
- `shape?: 'circle' | 'square' | 'rounded'` - Avatar shape
- `status?: 'online' | 'offline' | 'away' | 'busy'` - User status
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Avatar } from './components/Avatar';

<Avatar
  src="/user-avatar.jpg"
  alt="User Avatar"
  name="John Doe"
  size="lg"
  shape="circle"
  status="online"
/>
```

## Navigation Components

### Button

A versatile button component with various styles and states.

**Props:**
- `children: ReactNode` - Button content
- `variant?: 'primary' | 'secondary' | 'success' | 'warning' | 'danger' | 'ghost' | 'link'` - Button style
- `size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl'` - Button size
- `disabled?: boolean` - Whether button is disabled
- `loading?: boolean` - Whether button shows loading state
- `icon?: ReactNode` - Icon to display
- `iconPosition?: 'left' | 'right'` - Icon position
- `fullWidth?: boolean` - Whether button takes full width
- `onClick?: () => void` - Click handler
- `type?: 'button' | 'submit' | 'reset'` - Button type
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Button } from './components/Button';
import { PlusIcon, LoadingIcon } from './icons';

<Button
  variant="primary"
  size="md"
  icon={<PlusIcon />}
  iconPosition="left"
  loading={isLoading}
  onClick={handleClick}
>
  {isLoading ? 'Creating...' : 'Create New'}
</Button>
```

**Button Group Example:**
```jsx
import { ButtonGroup, Button } from './components';

<ButtonGroup>
  <Button variant="primary">Save</Button>
  <Button variant="secondary">Cancel</Button>
  <Button variant="danger">Delete</Button>
</ButtonGroup>
```

### Tabs

A tabbed interface component for organizing content.

**Props:**
- `tabs: Array<{id: string, label: string, content: ReactNode, disabled?: boolean}>` - Tab definitions
- `activeTab: string` - Currently active tab ID
- `onTabChange: (tabId: string) => void` - Tab change handler
- `variant?: 'default' | 'pills' | 'underline'` - Tab style variant
- `size?: 'sm' | 'md' | 'lg'` - Tab size
- `fullWidth?: boolean` - Whether tabs take full width
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Tabs } from './components/Tabs';

const [activeTab, setActiveTab] = useState('overview');

const tabs = [
  {
    id: 'overview',
    label: 'Overview',
    content: <div>Overview content</div>
  },
  {
    id: 'details',
    label: 'Details',
    content: <div>Details content</div>
  },
  {
    id: 'settings',
    label: 'Settings',
    content: <div>Settings content</div>
  }
];

<Tabs
  tabs={tabs}
  activeTab={activeTab}
  onTabChange={setActiveTab}
  variant="pills"
  size="md"
/>
```

### Breadcrumb

A navigation component showing the current page location.

**Props:**
- `items: Array<{label: string, href?: string, onClick?: () => void}>` - Breadcrumb items
- `separator?: ReactNode` - Separator between items
- `maxItems?: number` - Maximum number of items to show
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Breadcrumb } from './components/Breadcrumb';
import { ChevronRightIcon } from './icons';

const breadcrumbItems = [
  { label: 'Home', href: '/' },
  { label: 'Products', href: '/products' },
  { label: 'Electronics', href: '/products/electronics' },
  { label: 'Smartphones' }
];

<Breadcrumb
  items={breadcrumbItems}
  separator={<ChevronRightIcon />}
  maxItems={5}
/>
```

## Feedback Components

### Modal

A modal dialog component for overlays and forms.

**Props:**
- `isOpen: boolean` - Whether modal is visible
- `onClose: () => void` - Close handler
- `title?: string` - Modal title
- `children: ReactNode` - Modal content
- `size?: 'sm' | 'md' | 'lg' | 'xl' | 'full'` - Modal size
- `closeOnOverlayClick?: boolean` - Whether clicking overlay closes modal
- `closeOnEscape?: boolean` - Whether pressing escape closes modal
- `showCloseButton?: boolean` - Whether to show close button
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Modal, Button } from './components';

const [showModal, setShowModal] = useState(false);

<Modal
  isOpen={showModal}
  onClose={() => setShowModal(false)}
  title="Confirm Action"
  size="md"
  closeOnOverlayClick
  closeOnEscape
>
  <p>Are you sure you want to proceed with this action?</p>
  <div className="modal-actions">
    <Button variant="primary" onClick={handleConfirm}>
      Confirm
    </Button>
    <Button variant="secondary" onClick={() => setShowModal(false)}>
      Cancel
    </Button>
  </div>
</Modal>
```

### Toast

A notification component for displaying temporary messages.

**Props:**
- `message: string` - Toast message
- `type?: 'success' | 'warning' | 'error' | 'info'` - Toast type
- `duration?: number` - Auto-dismiss duration in milliseconds
- `onClose?: () => void` - Close handler
- `action?: ReactNode` - Action button
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Toast, Button } from './components';

const [toasts, setToasts] = useState([]);

const showToast = (message, type = 'info') => {
  const id = Date.now();
  const toast = {
    id,
    message,
    type,
    duration: 5000
  };
  
  setToasts(prev => [...prev, toast]);
  
  setTimeout(() => {
    setToasts(prev => prev.filter(t => t.id !== id));
  }, toast.duration);
};

// Usage
showToast('Operation completed successfully!', 'success');
showToast('Please check your input', 'warning');
showToast('An error occurred', 'error');
```

**Toast Container Example:**
```jsx
import { ToastContainer } from './components/ToastContainer';

<ToastContainer>
  {toasts.map(toast => (
    <Toast
      key={toast.id}
      message={toast.message}
      type={toast.type}
      onClose={() => setToasts(prev => prev.filter(t => t.id !== toast.id))}
    />
  ))}
</ToastContainer>
```

### Progress

A progress indicator component for showing completion status.

**Props:**
- `value: number` - Current progress value (0-100)
- `max?: number` - Maximum progress value (default: 100)
- `variant?: 'line' | 'circle' | 'semi-circle'` - Progress style
- `size?: 'sm' | 'md' | 'lg'` - Progress size
- `showLabel?: boolean` - Whether to show progress label
- `labelPosition?: 'top' | 'bottom' | 'left' | 'right'` - Label position
- `color?: string` - Custom progress color
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Progress } from './components/Progress';

const [progress, setProgress] = useState(0);

useEffect(() => {
  const interval = setInterval(() => {
    setProgress(prev => {
      if (prev >= 100) {
        clearInterval(interval);
        return 100;
      }
      return prev + 10;
    });
  }, 1000);

  return () => clearInterval(interval);
}, []);

<Progress
  value={progress}
  variant="line"
  size="lg"
  showLabel
  labelPosition="top"
  color="#4CAF50"
/>
```

## Utility Components

### Portal

A component for rendering content outside the normal DOM hierarchy.

**Props:**
- `children: ReactNode` - Content to render
- `container?: HTMLElement` - Container element (defaults to document.body)
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { Portal } from './components/Portal';

const [showTooltip, setShowTooltip] = useState(false);

<div
  onMouseEnter={() => setShowTooltip(true)}
  onMouseLeave={() => setShowTooltip(false)}
>
  Hover me
  {showTooltip && (
    <Portal>
      <div className="tooltip">
        This tooltip is rendered in a portal
      </div>
    </Portal>
  )}
</div>
```

### ClickOutside

A component that detects clicks outside of its children.

**Props:**
- `children: ReactNode` - Content to monitor
- `onClickOutside: () => void` - Handler for outside clicks
- `className?: string` - Additional CSS classes

**Example:**
```jsx
import { ClickOutside } from './components/ClickOutside';

const [showDropdown, setShowDropdown] = useState(false);

<ClickOutside onClickOutside={() => setShowDropdown(false)}>
  <div className="dropdown">
    <button onClick={() => setShowDropdown(!showDropdown)}>
      Toggle Dropdown
    </button>
    {showDropdown && (
      <div className="dropdown-menu">
        <div>Option 1</div>
        <div>Option 2</div>
      </div>
    )}
  </div>
</ClickOutside>
```

## Component Composition

### Higher-Order Components

#### `withLoading`

A HOC that adds loading state to components.

```jsx
import { withLoading } from './hocs/withLoading';

const UserList = ({ users, loading }) => {
  if (loading) {
    return <div>Loading users...</div>;
  }
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};

export default withLoading(UserList);
```

#### `withErrorBoundary`

A HOC that adds error boundary functionality.

```jsx
import { withErrorBoundary } from './hocs/withErrorBoundary';

const ErrorFallback = ({ error, resetError }) => (
  <div>
    <h2>Something went wrong</h2>
    <p>{error.message}</p>
    <button onClick={resetError}>Try again</button>
  </div>
);

export default withErrorBoundary(UserList, ErrorFallback);
```

### Render Props

#### `DataFetcher`

A component that fetches data and provides it to children.

```jsx
import { DataFetcher } from './components/DataFetcher';

<DataFetcher
  url="/api/users"
  render={({ data, loading, error }) => {
    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error.message}</div>;
    
    return (
      <ul>
        {data.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    );
  }}
/>
```

## Styling and Theming

### CSS Variables

Components use CSS custom properties for consistent theming:

```css
:root {
  /* Colors */
  --color-primary: #007bff;
  --color-secondary: #6c757d;
  --color-success: #28a745;
  --color-warning: #ffc107;
  --color-danger: #dc3545;
  --color-info: #17a2b8;
  
  /* Typography */
  --font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --font-size-sm: 0.875rem;
  --font-size-md: 1rem;
  --font-size-lg: 1.125rem;
  
  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  
  /* Border radius */
  --border-radius-sm: 0.25rem;
  --border-radius-md: 0.375rem;
  --border-radius-lg: 0.5rem;
  
  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}
```

### Theme Provider

A context provider for managing application themes:

```jsx
import { ThemeProvider, useTheme } from './components/ThemeProvider';

const App = () => {
  const [theme, setTheme] = useState('light');
  
  return (
    <ThemeProvider theme={theme} setTheme={setTheme}>
      <div className={`app app--${theme}`}>
        <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
          Toggle Theme
        </button>
        {/* App content */}
      </div>
    </ThemeProvider>
  );
};

const ThemedComponent = () => {
  const { theme, colors } = useTheme();
  
  return (
    <div style={{ backgroundColor: colors.background, color: colors.text }}>
      Current theme: {theme}
    </div>
  );
};
```

### Responsive Design

Components include responsive breakpoints:

```css
/* Mobile first approach */
.component {
  /* Base styles for mobile */
  padding: var(--spacing-sm);
  font-size: var(--font-size-sm);
}

/* Tablet and up */
@media (min-width: 768px) {
  .component {
    padding: var(--spacing-md);
    font-size: var(--font-size-md);
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .component {
    padding: var(--spacing-lg);
    font-size: var(--font-size-lg);
  }
}
```

### Accessibility

Components follow WCAG guidelines:

```jsx
// Proper ARIA labels
<Button
  aria-label="Close modal"
  aria-describedby="modal-description"
  onClick={onClose}
>
  ×
</Button>

// Keyboard navigation
<div
  role="button"
  tabIndex={0}
  onKeyDown={(e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      handleClick();
    }
  }}
  onClick={handleClick}
>
  Clickable div
</div>

// Focus management
useEffect(() => {
  if (isOpen) {
    const focusableElements = modalRef.current.querySelectorAll(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    const firstElement = focusableElements[0];
    if (firstElement) {
      firstElement.focus();
    }
  }
}, [isOpen]);
```