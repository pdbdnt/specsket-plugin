# No authoring kit

Part of [the rules for specific situations](ingestion-conditional-rules.md).

Without the authoring kit, fetch the `current` product schema through `specsket_get_entity_schema`; when product presentation is advertised, set `include_product_presentation_contracts` to true. The presentation contracts arrive compact by default: `{ "$ref": "#/shared_schemas/<name>" }` stands for the schema stored under `product_presentation_contracts.shared_schemas.<name>`; follow it when you need that shape, and set `presentation_contract_view` to `full` only if you cannot.
