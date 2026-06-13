export default async function handler(req, res) {
  res.setHeader('Access-Control-Allow-Origin', 'https://dohabotstudio.com')
  res.setHeader('Access-Control-Allow-Methods', 'POST, OPTIONS')
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type')
  if (req.method === 'OPTIONS') return res.status(200).end()
  if (req.method !== 'POST') return res.status(405).json({ error: 'Method not allowed' })

  const { name, email, company, message, phone } = req.body
  if (!name || !email || !message) {
    return res.status(400).json({ error: 'Name, email and message required' })
  }

  // Send via Formspree (set FORMSPREE_ID in Vercel env vars)
  try {
    const formspreeRes = await fetch(`https://formspree.io/f/${process.env.FORMSPREE_ID}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'Accept': 'application/json' },
      body: JSON.stringify({
        name,
        email,
        company: company || 'Not provided',
        phone: phone || 'Not provided',
        message,
        _subject: `New enquiry from ${name} — Doha Bot Studio`,
      }),
    })

    if (!formspreeRes.ok) throw new Error('Formspree failed')
    return res.status(200).json({ success: true, message: 'Message sent!' })
  } catch (err) {
    return res.status(500).json({ error: 'Failed to send: ' + err.message })
  }
}
