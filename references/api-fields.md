# Airbnb API Fields Reference

## Search Results (search_all)

Each listing in the search results contains:

| Field | Type | Description |
|-------|------|-------------|
| `room_id` | int/string | Unique listing ID, used for detail fetch |
| `name` | string | Listing title (localized) |
| `title` | string | Generic type like "Brighton and Hove 的房间" |
| `structuredContent.primaryLine` | list | Bedroom/bed/bathroom info items |
| `structuredContent.reviewSnippet` | list | One guest review highlight |
| `price.unit.amount` | float | Total price before discount |
| `price.break_down` | list | Price breakdown (discount, total) |
| `rating.value` | float | Rating 0-5 |
| `rating.reviewCount` | string | Number of reviews |
| `coordinates.latitude` | float | Listing latitude |
| `coordinates.longitud` | float | Listing longitude (note: typo in API) |
| `badges` | list | "TOP_X_GUEST_FAVORITE", "GUEST_FAVORITE", etc. |
| `images` | list | Photo URLs |
| `paymentMessages` | list | Payment-related messages |

### PrimaryLine Items

Each item in `structuredContent.primaryLine`:

```json
{"__typename": "MainSectionMessage", "body": "2 间卧室", "type": "BEDINFO"}
{"__typename": "MainSectionMessage", "body": "1 张床", "type": "BEDINFO"}
{"__typename": "MainSectionMessage", "body": "1.5 个卫生间", "type": "BATHROOMINFO"}
```

Types: `BEDINFO`, `BATHROOMINFO`

### Price Break Down

```json
{"description": "8 晚 x ￥407.98", "amount": 3263.0, "currency": "￥.80"}
{"description": "周租折扣", "amount": -321.0, "currency": "￥.77"}
{"description": "折后价格", "amount": 2942.0, "currency": "￥.03"}
```

To get total price after discount, find the item with "折后" in description.

## Detail Page (HTML scraping)

When fetching individual listing pages, extract:

| Field | Pattern | Description |
|-------|---------|-------------|
| JSON-LD | `<script type="application/ld+json">` | Name, description, images, rating, coordinates |
| roomType | `"roomType":"[^"]+"` | "Entire home/apt", "Private room", etc. |
| propertyType | `"propertyType":"[^"]+"` | "整套出租单元", "民居", etc. |
| description | From JSON-LD | Listing description text |

### JSON-LD Structure (schema.org VacationRental)

```json
{
  "@context": "https://schema.org",
  "@type": "VacationRental",
  "name": "...",
  "description": "...",
  "image": [...],
  "latitude": 50.82,
  "longitude": -0.13,
  "aggregateRating": {"ratingValue": 4.92, "ratingCount": "397"}
}
```

## Common Coordinates

| City | Center Lat | Center Lng |
|------|------------|------------|
| Brighton (UK) | 50.8225 | -0.1373 |
| London (UK) | 51.5074 | -0.1278 |
| Paris (France) | 48.8566 | 2.3522 |
| Barcelona (Spain) | 41.3851 | 2.1734 |
| Amsterdam (Netherlands) | 52.3676 | 4.9041 |
| Rome (Italy) | 41.9028 | 12.4964 |
| Berlin (Germany) | 52.5200 | 13.4050 |
| Lisbon (Portugal) | 38.7223 | -9.1393 |
| Prague (Czech Republic) | 50.0755 | 14.4378 |
| Vienna (Austria) | 48.2082 | 16.3738 |
