# Prompt Output Contracts

## Purpose

This document defines the minimum structured outputs and durable records used by Version One. These contracts keep long AI sessions from drifting and give implementation a stable handoff between auth, credits, intake, generation, artifacts, verification, and admin approval.

## Shared Context Envelope

Every user-owned generation contract must carry tenant context.

```json
{
  "schemaVersion": "context-v1",
  "userId": "uuid",
  "organizationId": "uuid",
  "storeId": "uuid | null",
  "generationRunId": "uuid | null",
  "actorRole": "user | super_admin",
  "source": "builder_ui | super_admin | fixture",
  "createdAt": "iso_datetime"
}
```

## Brand Prompt Intake Output

```json
{
  "schemaVersion": "brand-intake-v1",
  "status": "draft | needs_followup | ready_for_generation | rejected",
  "context": {
    "userId": "uuid",
    "organizationId": "uuid"
  },
  "source": {
    "rawPrompt": "",
    "submittedFields": {
      "brandName": "",
      "productType": "",
      "targetAudience": "",
      "region": "",
      "language": "",
      "constraints": ""
    }
  },
  "brand": {
    "name": "",
    "summary": "",
    "tone": [],
    "values": [],
    "marketPosition": "",
    "voiceGuidelines": {
      "copyStyle": "",
      "do": [],
      "avoid": []
    }
  },
  "audience": {
    "primarySegment": "",
    "secondarySegments": [],
    "needs": [],
    "objections": [],
    "buyingIntent": "",
    "accessibilityExpectations": []
  },
  "products": {
    "type": "",
    "category": "",
    "catalogSize": "single | small | medium | unknown",
    "seedProductCount": 0,
    "variantsNeeded": "yes | no | unknown",
    "variantTypes": [],
    "pricePositioning": "budget | mid_market | premium | luxury | unknown",
    "requiredProductFields": [],
    "categoryAssumptions": []
  },
  "store": {
    "requiredPages": ["home", "product_listing", "product_detail", "cart", "mock_checkout", "confirmation"],
    "contentPriorities": [],
    "trustElements": [],
    "checkoutMode": "mock_checkout",
    "currency": "",
    "language": "",
    "region": ""
  },
  "visualDirection": {
    "styleKeywords": [],
    "colorDirection": "",
    "imageryDirection": "",
    "layoutDirection": "",
    "avoid": []
  },
  "tenantControls": {
    "requiresApprovedUser": true,
    "requiresCredits": true,
    "creditEstimate": 0
  },
  "constraints": {
    "hard": [],
    "soft": [],
    "complianceNotes": [],
    "demoSafety": ["synthetic_data", "mock_checkout", "no_production_credentials"]
  },
  "facts": [],
  "assumptions": [],
  "unknowns": [],
  "openQuestions": [],
  "quality": {
    "completenessScore": 0,
    "confidence": "low | medium | high",
    "blockingMissingFields": [],
    "readyForStoreGeneration": false
  },
  "extensionPoints": {
    "creativeGeneration": { "enabledForVersionOne": false, "notes": [] },
    "campaignLaunch": { "enabledForVersionOne": false, "notes": [] },
    "visualBuilder": { "enabledForVersionOne": false, "notes": [] },
    "shopify": { "enabledForVersionOne": false, "notes": [] },
    "payments": { "enabledForVersionOne": false, "notes": ["Stripe and live payments are skipped."] }
  }
}
```

## Store Generation Plan Output

```json
{
  "schemaVersion": "store-generation-plan-v1",
  "status": "draft | ready_to_generate | generated | verification_failed | pending_admin_approval | demo_ready | rejected",
  "context": {
    "userId": "uuid",
    "organizationId": "uuid",
    "storeId": "uuid",
    "generationRunId": "uuid"
  },
  "input": {
    "brandIntakeBriefId": "uuid",
    "brandIntakeSchemaVersion": "brand-intake-v1",
    "briefSnapshot": {}
  },
  "credits": {
    "required": 0,
    "reserved": 0,
    "deducted": 0,
    "ledgerEntryIds": []
  },
  "template": {
    "baseTemplateKey": "",
    "categoryPresetKey": "",
    "selectionReason": "",
    "fallbackUsed": false
  },
  "storefront": {
    "pageMap": [
      {
        "key": "home | product_listing | product_detail | cart | mock_checkout | confirmation",
        "route": "",
        "required": true,
        "sections": []
      }
    ],
    "navigation": [],
    "contentBlocks": [],
    "responsiveRequirements": [],
    "accessibilityRequirements": []
  },
  "dataModel": {
    "entities": [],
    "relationships": [],
    "seedDataNeeds": [],
    "seedOrder": [],
    "persistenceMode": "json_fixture | supabase_dev"
  },
  "catalog": {
    "seedProductCount": 0,
    "requiredProductFields": [],
    "variantRules": [],
    "pricingRules": [],
    "inventoryRules": []
  },
  "cart": {
    "behaviors": [],
    "persistence": "",
    "identityRules": [],
    "edgeCases": []
  },
  "checkout": {
    "mode": "mock_checkout",
    "handoff": "",
    "paymentCollection": false,
    "testRequirements": []
  },
  "generatedArtifacts": {
    "storeConfig": "",
    "themeTokens": "",
    "catalogSeed": "",
    "copyBlocks": "",
    "generationReport": ""
  },
  "verification": {
    "requiredChecks": [],
    "results": [],
    "demoReady": false
  },
  "adminReview": {
    "required": true,
    "status": "not_requested | pending | approved | rejected",
    "reviewedBy": "uuid | null",
    "reviewedAt": "iso_datetime | null",
    "notes": ""
  },
  "extensionPoints": {
    "creativeGeneration": { "enabledForVersionOne": false, "notes": [] },
    "campaignLaunch": { "enabledForVersionOne": false, "notes": [] },
    "visualBuilder": { "enabledForVersionOne": false, "notes": [] },
    "shopify": { "enabledForVersionOne": false, "notes": [] },
    "payments": { "enabledForVersionOne": false, "notes": ["No Stripe/live payments in Version One."] }
  },
  "risks": [],
  "assumptions": [],
  "missingDecisions": [],
  "acceptanceCriteria": []
}
```

## Credit Ledger Event

```json
{
  "schemaVersion": "credit-ledger-event-v1",
  "id": "uuid",
  "organizationId": "uuid",
  "userId": "uuid",
  "adminActorId": "uuid | null",
  "generationRunId": "uuid | null",
  "type": "grant | reserve | deduct | refund | adjustment",
  "amount": 0,
  "balanceAfter": 0,
  "reason": "",
  "createdAt": "iso_datetime"
}
```

## AI Task Result

```json
{
  "schemaVersion": "ai-task-result-v1",
  "task": "normalize_brand | create_store_plan | generate_catalog | generate_copy | safety_review",
  "provider": "openai | mock | future_provider",
  "model": "",
  "status": "succeeded | failed | refused | fallback_used",
  "inputContractVersion": "",
  "outputContractVersion": "",
  "output": {},
  "errors": [],
  "warnings": [],
  "usage": {
    "estimatedCredits": 0,
    "inputTokens": 0,
    "outputTokens": 0
  }
}
```

## Rules

- Store Generation consumes `brand_intake_brief`; it must not re-read raw prompt history as the source of truth.
- All tenant-owned outputs must include user and organization context.
- Credits are admin-granted integer units, not currency.
- Checkout mode is `mock_checkout` only in Version One.
- Future extension fields must never activate payments, Shopify, campaigns, or visual editing by accident.
- AI outputs must be validated against schemas before persistence or rendering.
- A generated store is not demo-ready until verification passes and required admin approval is complete.
