One of the most common ways to connect to the Internet is by a wireless interface, mostly Wi-Fi. There are two types of network architectures used today: Cellular and Ad hoc/mesh. This lecture will cover the more common cellular network, and the next lecture will cover a mesh network in the form of Roofnet.

These wireless networks have to operate using electromagnetic waves to connect. This brings some interesting decisions that one would have to consider:
- Sharing - How do you share frequencies? Sending multiple signals on the same one will lead to obvious interference.
- Variability/adaptability - Rates typically change dynamically. How do you account for the changes due to distance or other factors?

Historically, wireless networks used infrared or radio waves as the transmission media, with radio being the more popular choice now.

How does the use of radio differ versus physical wires to send packets? One thing to consider is interference. Radio waves lack a method of isolation, whereas wires have some amount of physical shielding from other wires.

Radio waves can avoid interference by utilizing different frequencies. This is because $A \cos \omega t$ is orthogonal to various values of angular frequency $\omega$. In practice, sending at exact frequencies is difficult, so the better method is to send over a specific band of frequencies.

Management of these frequencies is done in two ways. Licensed bands are a set of frequencies granted to large telecom companies. These are strictly managed by the FCC or similar agencies in other countries. Unlicensed bands are commonly seen in wifi bands (2.4/5.0 GHz). Use of these bands are still managed by the FCC but they are not easily enforced. You need to get approval from the FCC to use certain frequencies if you However, it is illegal to jam these frequencies and the FCC will come after you.

# Sharing
On a wired network, sharing occurs in routers and is managed by congestion control algorithms. In a wireless setting, sharing occurs when multiple hosts transmit at the same frequency and same data. To address this, each layer has specific features that deal with redundancy or error correction
- The physical layer consists of how bits are represented and interpreted. The modulation defines this representation (usually as some form of sinusoidal representation), and the layer also includes some form of coding for redundancy.
- The Link layer above it contains additional error recovering, and it implements the MAC protocol, which determines which sender gets to transmit packets.
This design is easy to mess up.

The name "Cellular" comes from how the network is spaced out.
#drawing 
A "cell" is a section of the spectrum (band) that is assigned to communications. Each cell consists of a Base Station that manages connectivity (In wifi, it's called an access point). The goal is to limit interference between two neighboring cells. The easiest method to avoid interference is to run each cell on different bands such that no 2 neighbors share bands.

How do you allocate bandwidth in a cell? There are multiple possible methods to do this.
- Sending packets can be scheduled, similar to other scheduling algorithms. One example is Time Division Duplex (TDD), which is used in Bluetooth.
- Another method is called Carrier Sense Multiple Access
	- The general idea is to listen before transmitting. Each source listens to the channel, waiting until it is idle. When it is idle, each sender picks a random number in a specific window. The sender that wins the race sends. If 2 senders collide, then they double their window size and repeat.
Today, CSMA and TDD are used together in Wi-Fi. CSMA also has multiple variants.

# Variability and Adaptability
The intuition for sending packets is that transmissions on the physical layer is similar to sending a complex number.

Consider the complex number $e^{i\theta}=\cos \theta+i\sin \theta$.  The real part is called "in phase", and the imaginary part is the "quadrature". qam-4, or quadrature amplitude modulation, is the simplest method to implement this. This can be extended based on how many encoding of bits you desire.

An alternative to this method is Binary Phase-Shift Keying (bpsk). #TODO define. This can't be infinitely extended due to noise.
There are many other modulation and coding schemes today.

To deal with variable channel conditions, use a Bitrate Selection Algorithm. Historically, this used to be auto rate fallback, but the more modern algorithm is sample rate. Since cellular has less interference, the signal to noise ratio is more of a concern, and the algorithms are more controlled. There are even tables which map channel quality to the modulation algorithm used.