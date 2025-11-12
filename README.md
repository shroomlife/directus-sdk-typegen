# Directus SDK Types Generator

Opinionated CLI tool and library to generate TypeScript types from Directus collections to use with the Directus SDK.

Generates the individual interfaces for the Directus collections and a `Schema` type that collects all the interfaces together for use with the Directus SDK.

## Example

Generate the types from the CLI.

```bash
npx directus-sdk-typegen -u http://localhost:8055 -t your-token-here -o ./my-schema.ts
```

```typescript
// my-schema.ts
export interface Event {
	/** @required */
	id: string;
	status?: 'draft' | 'active' | 'past';
	sort?: number | null;
	user_created?: DirectusUser | string | null;
	date_created?: string | null;
	user_updated?: DirectusUser | string | null;
	date_updated?: string | null;
	/** @description Name of the event. @required */
	title: string;
	/** @required */
	slug: string;
	/** @description When does this event start? @required */
	start_at: string;
	/** @description When does this event end? */
	end_at?: string | null;
	/** @description Short description of the event. Used in SEO meta description. */
	description?: string | null;
	featured_image?: DirectusFile | string | null;
	timezone?: string | null;
	location?: 'whereby' | 'youtube' | 'other' | null;
	location_url?: string | null;
	location_youtube?: string | null;
	recording_url?: string | null;
	content?: string | null;
	location_whereby?: any | null;
	custom_registration_questions?: EventCustomRegistrationQuestion[] | string[];
	emails?: EventEmail[] | string[];
	feedback?: EventFeedback[] | string[];
	registrants?: EventRegistration[] | string[];
	hosts?: EventHost[] | string[];
}

export interface Schema {
	events: Event[]
}
```

Then use the generated types with the Directus SDK.

```typescript
import type { Schema } from './my-schema';

import {
	createDirectus,
    rest
} from '@directus/sdk';

const directus = createDirectus<Schema>('http://localhost:8055').with(rest())
```

## Installation

You can run the CLI tool using `npx` or install it globally.

```bash
npx directus-sdk-typegen -u http://localhost:8055 -t your-token-here -o ./my-schema.ts
```

or

```bash
npm install -g directus-sdk-typegen
```

```bash
directus-sdk-typegen -u http://localhost:8055 -t your-token-here -o ./my-schema.ts
```

## Usage

You can use the Directus Type Generator as a CLI tool or as a library in your Node.js application.

### CLI Usage

To generate TypeScript types from Directus collections, use the following command:

```bash
npx directus-sdk-typegen --url <DIRECTUS_API_URL> --token <DIRECTUS_API_TOKEN> --output <OUTPUT_FILE_PATH>
```

#### Options
- `-o, --output <path>`: Specify the output file path including filename (default: `./directus-schema.ts`).
- `-u, --url <url>`: Directus API URL (default: `http://localhost:8055`).
- `-t, --token <token>`: Directus API token. Needs Adminstrator permissions (default: `your-token-here`).
- `--type-prefix <prefix>`: Prefix to add to all generated type names (default: `''`). Useful for avoiding naming conflicts with other types in your project.

#### Example with Type Prefix

If you want to prefix all generated types with `Directus` to avoid naming conflicts:

```bash
npx directus-sdk-typegen --url http://localhost:8055 --token your-token-here --output ./my-schema.ts --type-prefix=Directus
```

This will generate types like `DirectusComponent`, `DirectusEvent`, etc., instead of `Component`, `Event`, etc. The prefix is also applied to all type references in relationships, ensuring type safety throughout your schema.

### Library Usage

You can also use the generator programmatically in your Node.js application:

```typescript
// Output to a file
import { generateDirectusTypes } from 'directus-sdk-typegen';

async function generateTypes() {
    try {
        await generateDirectusTypes({
            outputPath: './my-schema.ts',
            directusUrl: 'http://localhost:8055',
            directusToken: 'your-token-here',
            typePrefix: 'Directus', // Optional: prefix all type names
        });
    } catch (error) {
        console.error('Failed to generate types:', error);
    }
}

generateTypes();
```

```typescript
// Return types as a variable
import { generateDirectusTypes } from 'directus-sdk-typegen';

async function generateTypes() {
    try {
        const types = await generateDirectusTypes({
            directusUrl: 'http://localhost:8055',
            directusToken: 'your-token-here',
            typePrefix: 'Directus', // Optional: prefix all type names
        });
        // Do something else with the types
        console.log(types);
    } catch (error) {
        console.error('Failed to generate types:', error);
    }
}
            directusToken: 'your-token-here',
        });
        // Do something else with the types
        console.log(types);
    } catch (error) {
        console.error('Failed to generate types:', error);
    }
}
```


## Troubleshooting

### "unknown option '--type-prefix'" error

If you encounter an error like `error: unknown option '--type-prefix'`, this typically means one of the following:

1. **Using an outdated version from npm**: If you're running `bunx directus-sdk-typegen` or have globally installed the package, you might be using an older version that doesn't include the `--type-prefix` option. Make sure you're using version 0.2.1 or later:
   ```bash
   npx directus-sdk-typegen@latest --version
   ```

2. **Local repository not built**: If you're running from a cloned repository, make sure to build the TypeScript source after pulling updates:
   ```bash
   npm install
   npm run build
   ```

3. **Check your version**: Verify that you're using a version that includes this feature:
   ```bash
   directus-sdk-typegen --version
   # or
   npx directus-sdk-typegen --version
   ```

## Contributing

Happy to accept contributions that improve the types generated! But please open an issue first to discuss changes.

## License

This project is licensed under the MIT License.
