# Two-Hand Cockpit Duel (prototype)

Throwaway. Delete or absorb once the question below has an answer.

## Question

Is a 1v1 duel fun when each player can work only two controls at once (thruster levers, coupled bar, yaw dial, pitch dial, fire)?

## Files

- `index.html`: peer-to-peer build. Static, no server of ours. One player hosts and gets a 5-character room code; the other joins with it or with the `?join=CODE` invite link.
- `index-relay.html` + `server.ts`: fallback for pairs whose networks block direct WebRTC. `bun server.ts`, then expose port 8787 (e.g. `tailscale funnel 8787`).

## Peer-to-peer

- PeerJS (1.5.4, jsDelivr) and its free public broker introduce the two browsers; play traffic then goes directly between them over a WebRTC data channel.
- No TURN relay is configured, so some network pairs (strict NAT, some mobile carriers) cannot connect. Use the relay build for those.
- The host's page runs the match logic (`makeHost`): HP, kills, respawns, match phase. The host closing the tab ends the match.
- Both sides ping every second; 8 s of silence counts as a disconnect, because WebRTC does not reliably report a closed tab.

## Rules as built

- 2 hands. A held key, a drag, or a press of FIRE each uses one. Bar and a lever can't be held together.
- Shells: 80 m/s, gravity 9.81, inherit tank velocity. Reload 2 s.
- 2 hits per life, first to 3 kills, respawn after 3 s, 2 s spawn protection.

## Netcode (prototype grade)

- Each client simulates its own tank and sends pose at 20 Hz; the opponent is drawn 120 ms behind, interpolated.
- The shooter's client decides hits against what it sees and reports them. Cheatable by design.
- A background tab stops rendering, so a player who switches tabs freezes for the opponent.

## Verdict

_(fill in after play-testing)_
