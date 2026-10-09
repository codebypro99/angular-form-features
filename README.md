# Angular Form Feature Samples

Angular and TypeScript feature excerpts demonstrating reusable form controls, asynchronous autocomplete, local form persistence, and subscription cleanup.

## Purpose

These samples show how I separate form integration, option fetching, input behavior, and draft persistence into reusable components and services.

## Asynchronous Search Select

**Source:** [async-select-search.ts](./features/async-select-search.ts)

A custom search-select component built around Angular Material autocomplete and a reactive search control.

- Receives an autocomplete service through a required signal input.
- Exposes the service's option signal to its template.
- Debounces search-control changes by 300 milliseconds before invoking searches for text values.
- Maps a selected option to its underlying form value while displaying its label.
- Clears the underlying value when the search control becomes empty.
- Handles values written by the parent form without emitting another search-control change event.
- Uses a destruction notifier to terminate its value-change subscription.

**What this demonstrates:** connecting a custom form control to an asynchronous option source while separating displayed labels from stored values.

**Excerpt context:** the template, styles, option types, and auxiliary UI components are provided by the surrounding application.

## Autocomplete Data Service

**Source:** [base-autocomplete.service.ts](./features/base-autocomplete.service.ts)

An abstract service that fetches search results and maps them to selectable options.

- Injects Angular's HTTP client.
- Exposes options as a read-only signal.
- Accepts configurable value and label field names.
- Sends the search term through an HTTP POST request and extracts the response data.
- Provides overridable fetching and mapping methods for specialized behavior.

**What this demonstrates:** sharing option-loading logic across controls while allowing domain-specific customization.

**Behavior boundary:** requests are subscribed to independently; cancellation of previous searches and request-error handling are not implemented in this excerpt.

## Control Value Accessor Base

**Source:** [control-value-accessor.ts](./features/control-value-accessor.ts)

An abstract base component implementing Angular's `ControlValueAccessor` interface with an internal reactive control.

- Provides shared label, help, and error-display inputs.
- Connects to an optional local `NgControl` and assigns itself as its value accessor.
- Forwards internal value changes through the registered change callback.
- Writes external values without emitting internal change events.
- Tracks disabled state through a signal and enables or disables the internal control.
- Marks controls as touched and requests change detection after external writes.

**What this demonstrates:** centralizing recurring integration behavior for custom Angular form controls.

**Behavior boundary:** although `registerOnTouched` stores the supplied callback, this excerpt's own touch-marking implementation is replaced by that callback after registration. The sample documents the existing implementation rather than claiming complete touch-state behavior.

## Local Form Persistence

**Source:** [persist-form.directive.ts](./features/persist-form.directive.ts)

A standalone directive that restores and saves a supplied form under a configurable localStorage key.

- Restores saved JSON values with `patchValue` during initialization.
- Debounces form changes by 500 milliseconds before writing JSON to localStorage.
- Checks that a form and storage key were supplied.
- Catches and logs storage and JSON errors.
- Terminates its subscription when the directive is destroyed.

**What this demonstrates:** adding local draft persistence through a reusable directive instead of repeating persistence logic in each page.

**Behavior boundary:** this excerpt does not include expiration, schema migration, draft clearing after submission, or encryption. It persists emitted form values rather than using `getRawValue()` to include disabled controls.

## Subscription Cleanup Helper

**Source:** [until-destroyed.ts](./features/until-destroyed.ts)

A typed RxJS operator helper that applies `takeUntil` using a component's destruction notifier.

**What this demonstrates:** encapsulating a repeated subscription-cleanup pattern behind a small reusable function.

**Excerpt context:** notifier emission and completion are handled by the referenced `BaseComponent`, whose implementation is outside this collection.

## Technologies

Angular, TypeScript, Reactive Forms, Signals, Angular Material, HttpClient, RxJS, and browser localStorage.

## About This Collection

These files are feature excerpts for portfolio presentation and code review. Their original imports, selectors, and filenames are preserved. Shared base classes, response types, templates, styles, and application wiring belong to their surrounding applications.

Descriptions are based on source inspection. The excerpts have not been compiled or executed in isolation.

## Portfolio Message

I can organize reusable Angular form features around clear responsibilities: form integration, asynchronous option loading, user input, persistence, and lifecycle cleanup.
