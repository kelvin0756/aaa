# Functions Documentation

## Overview

This document provides comprehensive documentation for all utility functions, data processing functions, and helper functions in the codebase.

## Table of Contents

- [Data Validation](#data-validation)
- [Data Formatting](#data-formatting)
- [Data Transformation](#data-transformation)
- [String Utilities](#string-utilities)
- [Array Utilities](#array-utilities)
- [Object Utilities](#object-utilities)
- [Date Utilities](#date-utilities)
- [Math Utilities](#math-utilities)
- [Async Utilities](#async-utilities)
- [Storage Utilities](#storage-utilities)

## Data Validation

### `validateEmail(email: string): boolean`

Validates email format using regex pattern.

**Parameters:**
- `email`: Email string to validate

**Returns:** `boolean` - True if valid email format

**Example:**
```javascript
import { validateEmail } from './utils/validation';

const isValid = validateEmail('user@example.com'); // true
const isInvalid = validateEmail('invalid-email'); // false
```

### `validatePhone(phone: string): boolean`

Validates phone number format for international numbers.

**Parameters:**
- `phone`: Phone string to validate

**Returns:** `boolean` - True if valid phone format

**Example:**
```javascript
import { validatePhone } from './utils/validation';

const isValid = validatePhone('+1-555-123-4567'); // true
const isInvalid = validatePhone('123'); // false
```

### `validateURL(url: string): boolean`

Validates URL format and structure.

**Parameters:**
- `url`: URL string to validate

**Returns:** `boolean` - True if valid URL format

**Example:**
```javascript
import { validateURL } from './utils/validation';

const isValid = validateURL('https://example.com'); // true
const isInvalid = validateURL('not-a-url'); // false
```

### `validatePassword(password: string, options?: PasswordOptions): ValidationResult`

Validates password strength based on configurable criteria.

**Parameters:**
- `password`: Password string to validate
- `options`: Validation options (optional)

**Returns:** `ValidationResult` - Validation result with score and feedback

**Example:**
```javascript
import { validatePassword } from './utils/validation';

const result = validatePassword('MyPassword123!', {
  minLength: 8,
  requireUppercase: true,
  requireLowercase: true,
  requireNumbers: true,
  requireSpecialChars: true
});

console.log(result.isValid); // true
console.log(result.score); // 4
console.log(result.feedback); // ['Strong password']
```

## Data Formatting

### `formatCurrency(amount: number, currency: string, locale?: string): string`

Formats number as currency according to locale.

**Parameters:**
- `amount`: Numeric amount
- `currency`: Currency code (USD, EUR, etc.)
- `locale`: Locale string (optional, defaults to 'en-US')

**Returns:** `string` - Formatted currency string

**Example:**
```javascript
import { formatCurrency } from './utils/formatting';

const formatted = formatCurrency(1234.56, 'USD'); // "$1,234.56"
const euroFormatted = formatCurrency(1234.56, 'EUR', 'de-DE'); // "1.234,56 €"
```

### `formatDate(date: Date | string, format: string, locale?: string): string`

Formats date according to specified format and locale.

**Parameters:**
- `date`: Date object or date string
- `format`: Format string (e.g., 'YYYY-MM-DD', 'MM/DD/YYYY')
- `locale`: Locale string (optional)

**Returns:** `string` - Formatted date string

**Example:**
```javascript
import { formatDate } from './utils/formatting';

const date = new Date('2024-01-15');
const formatted = formatDate(date, 'YYYY-MM-DD'); // "2024-01-15"
const usFormatted = formatDate(date, 'MM/DD/YYYY'); // "01/15/2024"
const localized = formatDate(date, 'MMMM Do, YYYY', 'fr-FR'); // "15 janvier 2024"
```

### `formatNumber(number: number, options?: NumberFormatOptions): string`

Formats numbers with various options for display.

**Parameters:**
- `number`: Number to format
- `options`: Formatting options (optional)

**Returns:** `string` - Formatted number string

**Example:**
```javascript
import { formatNumber } from './utils/formatting';

const formatted = formatNumber(1234567.89, {
  decimals: 2,
  thousandsSeparator: ',',
  decimalSeparator: '.'
}); // "1,234,567.89"

const percentage = formatNumber(0.1234, { style: 'percent', decimals: 1 }); // "12.3%"
```

## Data Transformation

### `transformData(data: any, schema: Schema): any`

Transforms data according to schema definition.

**Parameters:**
- `data`: Raw data to transform
- `schema`: Transformation schema

**Returns:** `any` - Transformed data

**Example:**
```javascript
import { transformData } from './utils/transformation';

const schema = {
  id: 'user_id',
  name: 'full_name',
  email: 'email_address'
};

const rawData = {
  user_id: 123,
  full_name: 'John Doe',
  email_address: 'john@example.com'
};

const transformed = transformData(rawData, schema);
// Result: { id: 123, name: 'John Doe', email: 'john@example.com' }
```

### `flattenObject(obj: object, separator?: string): object`

Flattens nested objects into single-level objects.

**Parameters:**
- `obj`: Object to flatten
- `separator`: Separator for nested keys (default: '.')

**Returns:** `object` - Flattened object

**Example:**
```javascript
import { flattenObject } from './utils/transformation';

const nested = {
  user: {
    profile: {
      name: 'John',
      age: 30
    },
    settings: {
      theme: 'dark'
    }
  }
};

const flattened = flattenObject(nested);
// Result: { 'user.profile.name': 'John', 'user.profile.age': 30, 'user.settings.theme': 'dark' }
```

### `deepClone(obj: any): any`

Creates a deep copy of an object or array.

**Parameters:**
- `obj`: Object or array to clone

**Returns:** `any` - Deep cloned object or array

**Example:**
```javascript
import { deepClone } from './utils/transformation';

const original = { user: { name: 'John', preferences: { theme: 'dark' } } };
const cloned = deepClone(original);

cloned.user.preferences.theme = 'light';
console.log(original.user.preferences.theme); // 'dark'
console.log(cloned.user.preferences.theme); // 'light'
```

## String Utilities

### `capitalize(str: string): string`

Capitalizes the first letter of a string.

**Parameters:**
- `str`: String to capitalize

**Returns:** `string` - Capitalized string

**Example:**
```javascript
import { capitalize } from './utils/strings';

const result = capitalize('hello world'); // "Hello world"
```

### `camelCase(str: string): string`

Converts string to camelCase.

**Parameters:**
- `str`: String to convert

**Returns:** `string` - CamelCase string

**Example:**
```javascript
import { camelCase } from './utils/strings';

const result = camelCase('hello world'); // "helloWorld"
const result2 = camelCase('user-profile-settings'); // "userProfileSettings"
```

### `slugify(str: string): string`

Converts string to URL-friendly slug.

**Parameters:**
- `str`: String to convert

**Returns:** `string` - URL-friendly slug

**Example:**
```javascript
import { slugify } from './utils/strings';

const result = slugify('Hello World!'); // "hello-world"
const result2 = slugify('User Profile & Settings'); // "user-profile-settings"
```

### `truncate(str: string, length: number, suffix?: string): string`

Truncates string to specified length with optional suffix.

**Parameters:**
- `str`: String to truncate
- `length`: Maximum length
- `suffix`: Suffix to add (default: '...')

**Returns:** `string` - Truncated string

**Example:**
```javascript
import { truncate } from './utils/strings';

const result = truncate('This is a very long string', 20); // "This is a very long..."
const result2 = truncate('Short string', 20); // "Short string"
```

## Array Utilities

### `chunk(array: any[], size: number): any[][]`

Splits array into chunks of specified size.

**Parameters:**
- `array`: Array to chunk
- `size`: Size of each chunk

**Returns:** `any[][]` - Array of chunks

**Example:**
```javascript
import { chunk } from './utils/arrays';

const numbers = [1, 2, 3, 4, 5, 6, 7, 8];
const chunks = chunk(numbers, 3);
// Result: [[1, 2, 3], [4, 5, 6], [7, 8]]
```

### `unique(array: any[], key?: string): any[]`

Removes duplicates from array, optionally by key.

**Parameters:**
- `array`: Array to deduplicate
- `key`: Property key for object comparison (optional)

**Returns:** `any[]` - Array with unique values

**Example:**
```javascript
import { unique } from './utils/arrays';

const numbers = [1, 2, 2, 3, 3, 4];
const uniqueNumbers = unique(numbers); // [1, 2, 3, 4]

const users = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Jane' },
  { id: 1, name: 'John' }
];
const uniqueUsers = unique(users, 'id'); // [{ id: 1, name: 'John' }, { id: 2, name: 'Jane' }]
```

### `groupBy(array: any[], key: string): object`

Groups array items by specified key.

**Parameters:**
- `array`: Array to group
- `key`: Property key to group by

**Returns:** `object` - Grouped items

**Example:**
```javascript
import { groupBy } from './utils/arrays';

const users = [
  { id: 1, name: 'John', role: 'admin' },
  { id: 2, name: 'Jane', role: 'user' },
  { id: 3, name: 'Bob', role: 'admin' }
];

const grouped = groupBy(users, 'role');
// Result: {
//   admin: [{ id: 1, name: 'John', role: 'admin' }, { id: 3, name: 'Bob', role: 'admin' }],
//   user: [{ id: 2, name: 'Jane', role: 'user' }]
// }
```

## Object Utilities

### `pick(obj: object, keys: string[]): object`

Creates new object with only specified keys.

**Parameters:**
- `obj`: Source object
- `keys`: Array of keys to include

**Returns:** `object` - New object with selected keys

**Example:**
```javascript
import { pick } from './utils/objects';

const user = { id: 1, name: 'John', email: 'john@example.com', role: 'admin' };
const publicInfo = pick(user, ['id', 'name']); // { id: 1, name: 'John' }
```

### `omit(obj: object, keys: string[]): object`

Creates new object excluding specified keys.

**Parameters:**
- `obj`: Source object
- `keys`: Array of keys to exclude

**Returns:** `object` - New object without excluded keys

**Example:**
```javascript
import { omit } from './utils/objects';

const user = { id: 1, name: 'John', email: 'john@example.com', role: 'admin' };
const safeUser = omit(user, ['password', 'role']); // { id: 1, name: 'John', email: 'john@example.com' }
```

### `merge(target: object, ...sources: object[]): object`

Deep merges multiple objects.

**Parameters:**
- `target`: Target object
- `sources`: Source objects to merge

**Returns:** `object` - Merged object

**Example:**
```javascript
import { merge } from './utils/objects';

const defaultConfig = { theme: 'light', language: 'en' };
const userConfig = { theme: 'dark', notifications: true };
const finalConfig = merge(defaultConfig, userConfig);
// Result: { theme: 'dark', language: 'en', notifications: true }
```

## Date Utilities

### `addDays(date: Date, days: number): Date`

Adds specified number of days to date.

**Parameters:**
- `date`: Base date
- `days`: Number of days to add

**Returns:** `Date` - New date

**Example:**
```javascript
import { addDays } from './utils/dates';

const today = new Date();
const tomorrow = addDays(today, 1);
const nextWeek = addDays(today, 7);
```

### `isToday(date: Date): boolean`

Checks if date is today.

**Parameters:**
- `date`: Date to check

**Returns:** `boolean` - True if date is today

**Example:**
```javascript
import { isToday } from './utils/dates';

const today = new Date();
const yesterday = new Date(today);
yesterday.setDate(yesterday.getDate() - 1);

console.log(isToday(today)); // true
console.log(isToday(yesterday)); // false
```

### `formatRelativeTime(date: Date): string`

Formats date as relative time (e.g., "2 hours ago").

**Parameters:**
- `date`: Date to format

**Returns:** `string` - Relative time string

**Example:**
```javascript
import { formatRelativeTime } from './utils/dates';

const now = new Date();
const twoHoursAgo = new Date(now.getTime() - 2 * 60 * 60 * 1000);

console.log(formatRelativeTime(twoHoursAgo)); // "2 hours ago"
```

## Math Utilities

### `clamp(value: number, min: number, max: number): number`

Clamps value between min and max.

**Parameters:**
- `value`: Value to clamp
- `min`: Minimum value
- `max`: Maximum value

**Returns:** `number` - Clamped value

**Example:**
```javascript
import { clamp } from './utils/math';

const result = clamp(150, 0, 100); // 100
const result2 = clamp(-10, 0, 100); // 0
const result3 = clamp(50, 0, 100); // 50
```

### `roundToDecimal(value: number, decimals: number): number`

Rounds number to specified decimal places.

**Parameters:**
- `value`: Number to round
- `decimals`: Number of decimal places

**Returns:** `number` - Rounded number

**Example:**
```javascript
import { roundToDecimal } from './utils/math';

const result = roundToDecimal(3.14159, 2); // 3.14
const result2 = roundToDecimal(2.6789, 1); // 2.7
```

## Async Utilities

### `debounce(func: Function, delay: number): Function`

Creates debounced version of function.

**Parameters:**
- `func`: Function to debounce
- `delay`: Delay in milliseconds

**Returns:** `Function` - Debounced function

**Example:**
```javascript
import { debounce } from './utils/async';

const debouncedSearch = debounce((query) => {
  // Perform search
  console.log('Searching for:', query);
}, 300);

// Will only execute after 300ms of no calls
debouncedSearch('a');
debouncedSearch('ab');
debouncedSearch('abc'); // Only this executes
```

### `throttle(func: Function, delay: number): Function`

Creates throttled version of function.

**Parameters:**
- `func`: Function to throttle
- `delay`: Delay in milliseconds

**Returns:** `Function` - Throttled function

**Example:**
```javascript
import { throttle } from './utils/async';

const throttledScroll = throttle(() => {
  console.log('Scroll event handled');
}, 100);

// Will execute at most once every 100ms
window.addEventListener('scroll', throttledScroll);
```

### `retry(fn: Function, maxAttempts: number, delay: number): Promise<any>`

Retries function with exponential backoff.

**Parameters:**
- `fn`: Function to retry
- `maxAttempts`: Maximum retry attempts
- `delay`: Base delay in milliseconds

**Returns:** `Promise<any>` - Promise that resolves with function result

**Example:**
```javascript
import { retry } from './utils/async';

const fetchData = async () => {
  const response = await fetch('/api/data');
  if (!response.ok) throw new Error('Failed to fetch');
  return response.json();
};

const result = await retry(fetchData, 3, 1000);
```

## Storage Utilities

### `localStorage.set(key: string, value: any): void`

Stores value in localStorage with JSON serialization.

**Parameters:**
- `key`: Storage key
- `value`: Value to store

**Returns:** `void`

**Example:**
```javascript
import { localStorage } from './utils/storage';

localStorage.set('user', { id: 1, name: 'John' });
localStorage.set('theme', 'dark');
```

### `localStorage.get(key: string, defaultValue?: any): any`

Retrieves value from localStorage with JSON deserialization.

**Parameters:**
- `key`: Storage key
- `defaultValue`: Default value if key doesn't exist

**Returns:** `any` - Stored value or default

**Example:**
```javascript
import { localStorage } from './utils/storage';

const user = localStorage.get('user'); // { id: 1, name: 'John' }
const theme = localStorage.get('theme', 'light'); // 'dark' or 'light' if not set
```

### `sessionStorage.set(key: string, value: any): void`

Stores value in sessionStorage with JSON serialization.

**Parameters:**
- `key`: Storage key
- `value`: Value to store

**Returns:** `void`

**Example:**
```javascript
import { sessionStorage } from './utils/storage';

sessionStorage.set('tempData', { step: 2, formData: {} });
```

### `sessionStorage.get(key: string, defaultValue?: any): any`

Retrieves value from sessionStorage with JSON deserialization.

**Parameters:**
- `key`: Storage key
- `defaultValue`: Default value if key doesn't exist

**Returns:** `any` - Stored value or default

**Example:**
```javascript
import { sessionStorage } from './utils/storage';

const tempData = sessionStorage.get('tempData'); // { step: 2, formData: {} }
const currentStep = sessionStorage.get('currentStep', 1); // 2 or 1 if not set
```