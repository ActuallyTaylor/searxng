# Requirements and Test Oracles

## Functional Requirements

1. **FR-1:** The system shall process a user search query and return the results in a result container.  
   **Source:** `docs/src/searx.search.rst`, `searx/search/__init__.py`

2. **FR-2:** The system shall create search requests for all selected search engines that are available and successfully initialized.  
   **Source:** `searx/search/__init__.py`

3. **FR-3:** The system shall skip search engines that are unavailable or suspended.  
   **Source:** `searx/search/__init__.py`

4. **FR-4:** The system shall support search parameters including page number, safe search, time range, language, and engine-specific data.  
   **Source:** `searx/search/processors/abstract.py`

5. **FR-5:** The system shall skip an engine when the requested search condition is not supported by that engine, including unsupported paging, exceeding the maximum page, or unsupported time-range filtering.  
   **Source:** `searx/search/processors/abstract.py`

6. **FR-6:** The system shall send valid search requests to multiple selected engines and collect their responses in a shared result container.  
   **Source:** `searx/search/__init__.py`

7. **FR-7:** The system shall pass searches to initialized client-side plugins before and after searching
   **Source:** `searx/plugins/__init__.py`

7. **FR-8:** The system shall pass every response from every search engine into initialized client-side plugins.
   **Source:** `searx/plugins/__init__.py`

## Non-Functional Requirements

1. **NFR-1:** The system shall protect user privacy by not tracking users.  
   **Source:** `README.rst`

2. **NFR-2:** The system shall protect user privacy by not profiling users.  
   **Source:** `README.rst`

3. **NFR-3:** The system shall manage search request timeouts so that slow or unresponsive search engines do not prevent the overall search from completing.  
   **Source:** `searx/search/__init__.py`

## Test Oracles

| Requirement ID | Requirement Description | Test Oracle (Expected Behavior) |
|----------------|-------------------------|---------------------------------|
| FR-1 | Process a search query and return a result container. | After a valid search query is executed, the `search()` method should return a `ResultContainer`. |
| FR-2 | Create requests for selected and available engines. | When multiple valid engines are selected, the generated request list should contain a request for each available selected engine. |
| FR-3 | Skip unavailable or suspended engines. | If an engine is missing, failed during initialization, or is suspended, no search request should be created for that engine. |
| FR-4 | Support search parameters. | When a search query specifies values such as page number, safe search, time range, or language, those values should appear in the generated request parameters. |
| FR-5 | Skip unsupported search conditions. | If a query requests page 2 from an engine that does not support paging, exceeds the engine's maximum page, or uses an unsupported time range, `get_params()` should return `None`. |
| FR-6 | Send requests to multiple engines and collect results. | When several valid engine requests are created, each request should be executed and its output should update the shared result container. |
| FR-7 | Send search requests to initialized plugins before and after sending to search engines. | When a search is performed, the search should first pass through `Plugin.pre_search`, then into the search engine, then into `Plugin.post_search`. |
| FR-8 | Send each search result to initialized plugins. | When a search is completed, each search result should pass through a plugin with `Plugin.on_result` defined. |

| NFR-1 | Users shall not be tracked. | Normal search operations should not rely on a persistent mechanism that tracks an individual user's searches. |
| NFR-2 | Users shall not be profiled. | Search behavior should not depend on a persistent user profile created from previous search activity. |
| NFR-3 | Manage search request timeouts. | If an engine takes longer than the calculated allowed timeout, it should be marked as unresponsive due to timeout rather than blocking the search indefinitely. |

## Sources Used

- `README.rst`
- `docs/src/searx.search.rst`
- `docs/src/searx.search.processors.rst`
- `searx/search/__init__.py`
- `searx/search/processors/abstract.py`
