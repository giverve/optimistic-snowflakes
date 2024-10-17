# Optimistic Snowflakes

## Overview

**`optimistic-snowflakes`** is a simple, robust, and scalable unique ID generator designed for both frontend and backend systems. It generates 64-bit Snowflake IDs. These IDs are time-based, providing ordering by generation time, and are split into backend and frontend variants.

## Features

- **Backend Snowflake IDs**: Include machine ID and sequence for backend use.
- **Frontend Snowflake IDs**: Use randomness for lightweight ID generation on frontend environments.
- **Custom Epoch**: IDs are generated relative to a custom epoch.
- **Scalable**: Supports up to 10-bit machine IDs and 12-bit sequences for the backend.
- **Compact**: 64-bit integers ensure IDs are efficient and small.
- **Time-based**: IDs are ordered by their generation time.

## Installation

To use `optimistic-snowflakes` in your project, you can install it via npm:

```bash
npm install optimistic-snowflakes
```

## Usage

### Backend Snowflake Generator

To generate IDs on the backend, use the SnowflakeGenerator class. You must provide a machineId, a unique identifier for the machine generating the IDs.

```typescript
import { SnowflakeGenerator } from "optimistic-snowflakes";

const generator = new SnowflakeGenerator(BigInt(1)); // Machine ID: 1
const id = generator.generate();

console.log(id.toString()); // Example output: '9223372036854775807'
```

### Frontend Snowflake Generator

For frontend applications, the generateFrontendSnowflakeId function generates IDs using randomness and the current timestamp.

```typescript
import { generateFrontendSnowflakeId } from "optimistic-snowflakes";

const id = generateFrontendSnowflakeId();

console.log(id.toString()); // Example output: '9223372036854775807'
```

### Extracting SnowflakeId

You can extract and inspect components of a Snowflake ID using the extractSnowflakeId function. It provides the timestamp, source (frontend or backend), and additional details based on the ID type.

```typescript
import { extractSnowflakeId } from "optimistic-snowflakes";

const info = extractSnowflakeId(id);

console.log(info);
// Example output:
// {
//   timestamp: '2024-10-10T08:45:27.000Z',
//   source: 'backend',
//   machineId: 1,
//   sequence: 123
// }
```

### Validating Snowflake IDs

The isValidSnowflakeId function ensures the integrity of a given ID. It checks for a valid timestamp, flag, machine ID, sequence, or randomness based on whether the ID is frontend or backend generated.

```typescript
import { isValidSnowflakeId } from "optimistic-snowflakes";

const isValid = isValidSnowflakeId(id);
console.log(isValid); // true or false
```

## Example

```typescript
import {
  SnowflakeGenerator,
  generateFrontendSnowflakeId,
  extractSnowflakeId,
  isValidSnowflakeId,
} from "optimistic-snowflakes";

// Backend
const backendGenerator = new SnowflakeGenerator(BigInt(1));
const backendId = backendGenerator.generate();
console.log(`Backend ID: ${backendId}`);

// Frontend
const frontendId = generateFrontendSnowflakeId();
console.log(`Frontend ID: ${frontendId}`);

// Extract components
const backendSnowflakeIdData = extractSnowflakeId(backendId);
const frontendSnowflakeIdData = extractSnowflakeId(frontendId);

console.log("Backend Components:", backendSnowflakeIdData);
console.log("Frontend Components:", frontendSnowflakeIdData);

// Validate IDs
console.log(`Is valid backend ID: ${isValidSnowflakeId(backendId)}`);
console.log(`Is valid frontend ID: ${isValidSnowflakeId(frontendId)}`);
```
