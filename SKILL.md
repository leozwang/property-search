# property-search

<description>
Expertise in generating accurate and practical OData 4.0 queries for the MLS Listings API, including domain knowledge about real estate fields like ListingContractDate, ClosePrice, etc. Use this skill whenever you need to construct a query for the `property_search` tool.
</description>

<instructions>
When constructing OData 4.0 queries for the MLS API, strictly adhere to the following guidelines:

## Domain Knowledge & Field Usage
- **Active**: The listing is on market and an offer has not been accepted.
- **ActiveUnderContract**: Is contingent, an offer has been accepted but the listing is still on market. Accepting Backup Offers, Backup Offer, Active with Accepted Offer, or Backup.
- **Canceled**: The listing contract has been terminated.
- **Closed**: Sold.
- **Delete**: The listing contract was never valid or other reason for the contract to be nullified.
- **Expired**: Expired.
- **Hold**: is "Hold Do Not Show" or "Temp Off Market". The listing is off market, but a contract still exists between the seller and the listing member and the listing is expected to come back on market. This is commonly done for renovations or other pre-showing preparation. For systems that don't use Hold, use Withdrawn. For systems that use both Hold and Withdrawn, Hold may be interpreted as an off market with the intention to return back on market.
- **Pending**: is Pending (Do Not Show): Offer Accepted,
- **Under Contract. An offer has been accepted and the listing is no longer on market.
Withdrawn Withdrawn**: The listing has been withdrawn from the market, but a contract still exists between the seller and the listing member. For those systems that use both Hold and Withdrawn, Withdrawn may represent an intention not to bring the listing back on the market. When Hold is not used by the system, Withdrawn does not represent any intention of returning to market or not.

- **ListingContractDate**: Date a seller signs a legally binding agreement with an agent.
- **OriginalOnMarketDate**: Exact date a property was first listed for sale on the MLS.
- **OnMarketDate**: Specific date a property is officially listed as "Active" and available for showings.
- **PurchaseContractDate**: Precise day the last party signs and delivers the finalized agreement.
- **CloseDate**: Official date ownership transfers to the buyer.
- **OriginalListPrice**: Initial asking price set by the seller.
- **ListPrice**: Advertised, current starting price for negotiations.
- **ClosePrice**: Final actual amount for which a property was sold.

### Check open house
- **OpenHouseYN**: This field returns a simple "true" to indicating the property has an open house scheduled.
- **OpenHouseSchedule**: If OpenHouseYN is "Yes", this field provides a list of the scheduled open houses, detailing the date, time, and host (e.g., {"OHDateTime":"3/7 1:00 pm - 4:00 pm","OHHostedBy":"Alex","OHOpenHouseID":83071181}). A sample list is shown below
    {"OHDateTime":"3/7 1:00 pm - 4:00 pm","OHHostedBy":"Alex","OHOpenHouseID":83071181},
    {"OHDateTime":"3/8 1:00 pm - 4:00 pm","OHHostedBy":"Bob","OHOpenHouseID":83071183},

## Query Construction Rules
1. **No $filter Prefix**: DO NOT add "$filter=" into the query string. The underlying library will append this automatically. Provide only the condition expression itself (e.g., `StandardStatus eq ResourceEnums.StandardStatus'Active'`).
2. **Field Selection & Data Formatting Validation**:
   - **CRITICAL STEP**: All information needed for validation is embedded in the description of the `query` parameter for the `property_search` tool. You MUST use this embedded information. DO NOT attempt to read local files.
   - The `query` parameter's description contains three critical pieces of information:
     1. **`defined fields (from mls.json)`**: The complete schema of all available fields and their data types.
     2. **`practically used fields (from valid.json)`**: A smaller, more reliable subset of fields that you should prioritize.
     3. **`Sample data (from sample.json)`**: A JSON object demonstrating the exact data formats for fields. You must adhere to these formats (e.g., `YYYY-MM-DD` for dates).
   - Your validation process is critical: First, check the `practically used fields`. If a field isn't there, check the `defined fields`. Finally, always verify the data format against the `Sample data`.
3. **Field Meaningfulness**: Do not use unreliable fields like `DaysOnMarket`. Instead, use reliable proxies (e.g., to filter by time on the market, you must use `OnMarketDate` and `CloseDate`).
4. **OData 4.0 Syntax**: Ensure all queries strictly adhere to the OData 4.0 specification.
   - Use correct logical operators (`eq`, `ne`, `gt`, `ge`, `lt`, `le`, `and`, `or`, `not`).
   - Include proper DateTime offset values for date and time fields.
   - Use proper string quoting and escaping.
5. **Verify Data Types**: Before using a field in a query, check its data type in the `defined fields` (from the `query` parameter's description). Special attention is required for enumerated types. For example, if the schema shows the `City` field is of type `ResourceEnums.City`, you must use that syntax.
   - **Correct:** `City eq ResourceEnums.City'Cupertino'`
   - **Incorrect:** `City eq 'Cupertino'`
   - **Correct:** `StandardStatus eq ResourceEnums.StandardStatus'Active'`
   - **Incorrect:** `StandardStatus eq 'Active'`
   Failing to use the correct syntax for the data type will result in an empty or failed query.

## Handling Outputs
The `property_search` tool returns a structured JSON object. To ensure consistent and effective analysis, you should always handle the output as follows:

1. **Save to File**: Always save the entire JSON output to a temporary file. This provides a stable record of the data and prevents clutter in the console.
2. **Analyze from File**: Read the JSON content from the temporary file to perform your analysis. The JSON object contains two main fields:
   - **`total_properties`**: An integer indicating the total number of matching properties. Check this first to understand the result size.
   - **`properties`**: An array containing the detailed property listings.
3. **Process Data**: Iterate through the `properties` array from the parsed file content to extract details and formulate your answer. This approach is mandatory for all outputs, regardless of their size, to ensure a robust workflow.
</instructions>
