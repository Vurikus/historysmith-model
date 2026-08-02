---
name: historysmith-model
description: Use when editing this repository, especially protobuf contracts under proto/ or shared game model data under model/.
---

# Historysmith Model

Use this skill before making changes in this repository.

## Repository roles

- `proto/` contains protobuf contracts used for exchange between the game client and server.
- `proto/common/` contains the top-level request/response abstraction. All transport messages must be wrapped in `EnvelopeRequestProto` from `proto/common/envelop_req.proto` or `EnvelopeResponseProto` from `proto/common/envelop_resp.proto`.
- `model/` contains the single shared description of game model data so client and server use the same numbers and model definitions.

## Proto message categories

Every concrete protobuf transport message belongs to one of these categories:

- `action`: a concrete player action, such as moving a squad or constructing a building.
- `syncstate`: a client request for the current state of an object.
- `intention`: a request that represents an attempted action or intent, such as hovering an attack button to check whether attacking is possible.
- `worldbase`: requests related to the game world.

When adding a concrete protobuf file for one of these categories, place it in the matching package directory under `proto/`: `proto/action/`, `proto/syncstate/`, `proto/intention/`, or `proto/worldbase/`.

## Envelope rules

- Add only top-level aggregation and envelope payload messages to `proto/common/`.
- Make new request flows reachable through `EnvelopeRequestProto`.
- Make new response flows reachable through `EnvelopeResponseProto`.
- Keep category-specific details out of `proto/common/`; `proto/common/` should route and wrap them.

## Model rules

- Put shared game model data in `model/`, organized by domain.
- Treat `model/` data as the common source of truth for both client and server.
- Do not duplicate gameplay constants or model definitions in protobuf files unless they are required for transport shape.

## Validation

- After changing protobuf files, run `./validate-proto.sh` when possible.
- Preserve existing package naming and protobuf style unless a broader migration is explicitly requested.
