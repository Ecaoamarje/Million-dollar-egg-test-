'use client'

import { useState, useEffect, useRef } from 'react'
import Image from 'next/image'
import Link from 'next/link'
import { motion, useScroll, useTransform, AnimatePresence, useInView } from 'framer-motion'
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { 
  Diamond, 
  Coins, 
  Users, 
  Gift, 
  ChevronRight, 
  MessageCircle,
  Twitter,
  Send,
  Sparkles,
  Crown,
} from 'lucide-react'

const GoldGradientText = ({ children, className = "" }) => (
  <span className={`bg-gradient-to-r from-[#FFD700] via-[#FDB931] to-[#BD9732] bg-clip-text text-transparent ${className}`}>
    {children}
  </span>
)

const LuxuryCard = ({ children, className = "" }) => (
  <div className={`relative group ${className}`}>
    <div className="absolute inset-0 bg-gradient-to-r from-[#FFD700] via-[#FDB931] to-[#BD9732] opacity-0 group-hover:opacity-100 blur-xl transition-opacity duration-300" />
    <div className="relative bg-black/80 backdrop-blur-sm border border-[#BD9732] p-8 rounded-xl hover:border-[#FFD700] transition-all duration-300">
      {children}
    </div>
  </div>
)

const CountdownTimer = () => {
  const calculateTimeLeft = () => {
    const launchDate = new Date('2024-12-31T00:00:00')
    const difference = +launchDate - +new Date()
    
    if (difference <= 0) {
      return { days: 0, hours: 0, minutes: 0, seconds: 0 }
    }
    
    return {
      days: Math.floor(difference / (1000 * 60 * 60 * 24)),
      hours: Math.floor((difference / (1000 * 60 * 60)) % 24),
      minutes: Math.floor((difference / 1000 / 60) % 60),
      seconds: Math.floor((difference / 1000) % 60)
    }
  }

  const [timeLeft, setTimeLeft] = useState(calculateTimeLeft())

  useEffect(() => {
    const timer = setInterval(() => {
      setTimeLeft(calculateTimeLeft())
    }, 1000)

    return () => clearInterval(timer)
  }, [])

  return (
    <div className="grid grid-cols-4 gap-4 max-w-2xl mx-auto">
      {Object.entries(timeLeft).map(([key, value]) => (
        <motion.div
          key={key}
          className="bg-black/40 backdrop-blur-sm border border-[#BD9732] p-4 rounded-xl"
          initial={{ scale: 0.9, opacity: 0 }}
          animate={{ scale: 1, opacity: 1 }}
          transition={{ duration: 0.5 }}
        >
          <motion.div
            className="text-4xl md:text-5xl font-bold mb-1"
            key={value}
            initial={{ y: -20, opacity: 0 }}
            animate={{ y: 0, opacity: 1 }}
            transition={{ duration: 0.5 }}
          >
            <GoldGradientText>{value.toString().padStart(2, '0')}</GoldGradientText>
          </motion.div>
          <div className="text-[#BD9732] text-sm md:text-base capitalize">{key}</div>
        </motion.div>
      ))}
    </div>
  )
}

const FloatingEggs = () => {
  return (
    <div className="absolute inset-0 overflow-hidden pointer-events-none">
      {[...Array(5)].map((_, i) => (
        <motion.div
          key={i}
          className="absolute"
          initial={{
            x: `${Math.random() * 100}%`,
            y: `${Math.random() * 100}%`,
            scale: Math.random() * 0.5 + 0.5,
            opacity: 0.3,
          }}
          animate={{
            y: [`${Math.random() * 100}%`, `${Math.random() * 100}%`],
            x: [`${Math.random() * 100}%`, `${Math.random() * 100}%`],
            rotate: [0, 360],
          }}
          transition={{
            duration: Math.random() * 20 + 10,
            repeat: Infinity,
            repeatType: "reverse",
          }}
        >
          <Image
            src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/IMG_2381-DZhRKXdVXq3QZNK5KhNZO40jeArN2o.png"
            alt="Floating Egg"
            width={100}
            height={120}
            className="opacity-20"
          />
        </motion.div>
      ))}
    </div>
  )
}

const NFTCard = ({ nft, index }) => {
  const cardRef = useRef(null)
  const isInView = useInView(cardRef, { once: true })

  return (
    <motion.div
      ref={cardRef}
      initial={{ opacity: 0, y: 50 }}
      animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 50 }}
      transition={{ duration: 0.5, delay: index * 0.1 }}
    >
      <LuxuryCard className="h-full">
        <motion.div
          className="relative mb-4 aspect-square overflow-hidden rounded-lg"
          whileHover={{ scale: 1.05 }}
          transition={{ type: "spring", stiffness: 300 }}
        >
          <Image
            src={nft.image}
            alt={nft.name}
            fill
            className="object-cover"
          />
          <motion.div
            className="absolute inset-0 bg-gradient-to-t from-black to-transparent opacity-0"
            initial={{ opacity: 0 }}
            whileHover={{ opacity: 1 }}
          />
          <motion.div
            className="absolute bottom-0 left-0 right-0 p-4 text-white"
            initial={{ y: 20, opacity: 0 }}
            whileHover={{ y: 0, opacity: 1 }}
          >
            <p className="text-sm font-bold">{nft.description}</p>
          </motion.div>
        </motion.div>
        <h3 className="text-2xl font-bold mb-2">
          <GoldGradientText>{nft.name}</GoldGradientText>
        </h3>
        <div className="flex justify-between items-center mb-4">
          <span className="text-[#BD9732]">{nft.tier}</span>
          <span className="text-[#FFD700]">Supply: {nft.supply}</span>
        </div>
        <div className="flex justify-between items-center">
          <span className="text-xl font-bold">
            <GoldGradientText>{nft.price}</GoldGradientText>
          </span>
          <motion.div whileHover={{ scale: 1.05 }} whileTap={{ scale: 0.95 }}>
            <Button className="bg-gradient-to-r from-[#FFD700] via-[#FDB931] to-[#BD9732] text-black hover:opacity-90">
              Mint Now
            </Button>
          </motion.div>
        </div>
      </LuxuryCard>
    </motion.div>
  )
}

const VIPGoldenEggCard = () => {
  const cardRef = useRef(null)
  const isInView = useInView(cardRef, { once: true })

  return (
    <motion.div
      ref={cardRef}
      initial={{ opacity: 0, y: 50 }}
      animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 50 }}
      transition={{ duration: 0.8 }}
      className="relative overflow-hidden rounded-3xl bg-gradient-to-r from-[#FFD700] via-[#FDB931] to-[#BD9732] p-1"
    >
      <div className="bg-black rounded-3xl p-8 relative z-10 overflow-hidden">
        <div className="absolute inset-0 bg-gradient-to-r from-[#FFD700]/10 via-[#FDB931]/10 to-[#BD9732]/10 opacity-50" />
        <motion.div
          initial={{ rotate: 0 }}
          animate={{ rotate: 360 }}
          transition={{ duration: 20, repeat: Infinity, ease: "linear" }}
          className="absolute inset-0 opacity-30"
          style={{
            backgroundImage: "url('data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23BD9732' fill-opacity='0.4'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E')",
          }}
        />
        <div className="relative z-10">
          <motion.div
            initial={{ x: -50, opacity: 0 }}
            animate={isInView ? { x: 0, opacity: 1 } : { x: -50, opacity: 0 }}
            transition={{ duration: 0.5, delay: 0.2 }}
            className="flex items-center justify-between mb-6"
          >
            <h3 className="text-3xl font-bold">
              <GoldGradientText>VIP Golden Egg</GoldGradientText>
            </h3>
            <Crown className="w-10 h-10 text-[#FFD700]" />
          </motion.div>
          <motion.p
            initial={{ y: 20, opacity: 0 }}
            animate={isInView ? { y: 0, opacity: 1 } : { y: 20, opacity: 0 }}
            transition={{ duration: 0.5, delay: 0.4 }}
            className="text-[#BD9732] mb-6"
          >
            Experience unparalleled luxury and exclusive benefits with our VIP Golden Egg membership.
          </motion.p>
          <motion.ul
            initial={{ y: 20, opacity: 0 }}
            animate={isInView ? { y: 0, opacity: 1 } : { y: 20, opacity: 0 }}
            transition={{ duration: 0.5, delay: 0.6 }}
            className="space-y-2 text-[#BD9732] mb-8"
          >
            {[
              { icon: <Diamond className="w-5 h-5 mr-2 text-[#FFD700]" />, text: "Early access to limited edition NFTs" },
              { icon: <Coins className="w-5 h-5 mr-2 text-[#FFD700]" />, text: "Enhanced staking rewards" },
              { icon: <Users className="w-5 h-5 mr-2 text-[#FFD700]" />, text: "Exclusive VIP community events" },
              { icon: <Gift className="w-5 h-5 mr-2 text-[#FFD700]" />, text: "Surprise airdrops and bonuses" },
            ].map((item, index) => (
              <motion.li
                key={index}
                initial={{ x: -20, opacity: 0 }}
                animate={isInView ? { x: 0, opacity: 1 } : { x: -20, opacity: 0 }}
                transition={{ duration: 0.5, delay: 0.8 + index * 0.1 }}
                className="flex items-center"
              >
                {item.icon}
                {item.text}
              </motion.li>
            ))}
          </motion.ul>
          <motion.div
            initial={{ y: 20, opacity: 0 }}
            animate={isInView ? { y: 0, opacity: 1 } : { y: 20, opacity: 0 }}
            transition={{ duration: 0.5, delay: 1.2 }}
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
          >
            <Button className="w-full bg-gradient-to-r from-[#FFD700] via-[#FDB931] to-[#BD9732] text-black hover:opacity-90 text-lg font-semibold py-6">
              Upgrade to VIP
            </Button>
          </motion.div>
        </div>
      </div>
    </motion.div>
  )
}

export default function Component() {
  const { scrollY } = useScroll()
  const y = useTransform(scrollY, [0, 300], [0, 150])
  const opacity = useTransform(scrollY, [0, 300], [1, 0])

  const features = [
    {
      icon: <Diamond className="w-8 h-8" />,
      title: "Exclusive NFT Collection",
      description: "Limited edition digital assets with unique properties and privileges"
    },
    {
      icon: <Coins className="w-8 h-8" />,
      title: "Crypto Egg Token",
      description: "100,000,000 initial supply for community governance and future developments"
    },
    {
      icon: <Users className="w-8 h-8" />,
      title: "Community Driven",
      description: "Join a movement that celebrates luxury, innovation, and collective power"
    },
    {
      icon: <Gift className="w-8 h-8" />,
      title: "Charitable Initiatives",
      description: "Supporting mental health awareness, education, and social impact"
    }
  ]

  const nftCollections = [
    {
      name: "Diamond Egg",
      tier: "Legendary",
      supply: "10",
      image: "https://hebbkx1anhila5yf.public.blob.vercel-storage.com/IMG_2351-BbRLpsmwTmkJCkxFghN981vT0eem9q.png",
      price: "100 ETH",
      description: "The rarest and most valuable egg in the collection. Grants access to exclusive events and premium features."
    },
    {
      name: "Golden Egg",
      tier: "Epic",
      supply: "100",
      image: "https://hebbkx1anhila5yf.public.blob.vercel-storage.com/IMG_2352-cCVlOxvmoT2jZEHybLiAXSxSQr5xGX.png",
      price: "10 ETH",
      description: "A highly sought-after egg with enhanced staking rewards and special community privileges."
    },
    {
      name: "Silver Egg",
      tier: "Rare",
      supply: "1,000",
      image: "https://hebbkx1anhila5yf.public.blob.vercel-storage.com/IMG_2349-CvzRPnP3TRCqfpkurQzsRHFSLxwpQM.png",
      price: "1 ETH",
      description: "A beautiful and rare egg that offers unique benefits and voting rights in the DAO."
    }
  ]

  return (
    <div className="min-h-screen bg-black text-white">
      <div className="fixed inset-0 bg-[radial-gradient(ellipse_at_center,_var(--tw-gradient-stops))] from-[#BD9732]/20 via-black to-black" />
      <FloatingEggs />

      <header className="fixed w-full z-50 bg-black/90 backdrop-blur-sm border-b border-[#BD9732]">
        <div className="container mx-auto px-4">
          <div className="flex items-center justify-between h-24">
            <Link href="/" className="text-4xl font-bold">
              <GoldGradientText>MDE$</GoldGradientText>
            </Link>
            <nav className="hidden md:flex items-center gap-8">
              {['About', 'NFT', 'VIP', 'Community'].map((item) => (
                <Link
                  key={item}
                  href={`#${item.toLowerCase()}`}
                  className="text-[#BD9732] hover:text-[#FFD700] transition-colors text-lg font-medium relative group"
                >
                  {item}
                  <span className="absolute -bottom-1 left-0 w-0 h-0.5 bg-gradient-to-r from-[#FFD700] to-[#BD9732] group-hover:w-full transition-all duration-300" />
                </Link>
              ))}
            </nav>
            <Button className="bg-gradient-to-r from-[#FFD700] via-[#FDB931] to-[#BD9732] text-black hover:opacity-90 font-semibold">
              Connect Wallet
            </Button>
          </div>
        </div>
      </header>

      <main>
        <section className="relative min-h-screen flex items-center justify-center pt-24">
          <motion.div style={{ y, opacity }} className="container mx-auto px-4 text-center">
            <motion.div
              initial={{ opacity: 0, scale: 0.8 }}
              animate={{ opacity: 1, scale: 1 }}
              transition={{ duration: 1.5, ease: "easeOut" }}
              className="relative mb-12"
            >
              <div className="absolute inset-0 bg-gradient-to-r from-[#FFD700]/20 via-transparent to-[#BD9732]/20 blur-3xl" />
              <Image
                src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/IMG_2381-DZhRKXdVXq3QZNK5KhNZO40jeArN2o.png"
                alt="Million Dollar Egg"
                width={500}
                height={600}
                className="mx-auto relative z-10"
                priority
              />
              <motion.div
                className="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2"
                animate={{
                  scale: [1, 1.2, 1],
                  rotate: [0, 360],
                }}
                transition={{
                  duration: 5,
                  ease: "easeInOut",
                  times: [0, 0.5, 1],
                  repeat: Infinity,
                }}
              >
                <Sparkles className="w-16 h-16 text-[#FFD700]" />
              </motion.div>
            </motion.div>
            
            <motion.div
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8, delay: 0.5 }}
              className="space-y-8"
            >
              <h1 className="text-6xl md:text-8xl font-bold">
                <GoldGradientText>Million Dollar Egg</GoldGradientText>
              </h1>
              <p className="text-2xl md:text-3xl text-[#BD9732] max-w-3xl mx-auto">
                The World's Most Luxurious Digital Asset
              </p>
              <div className="mb-12">
                <h2 className="text-3xl font-bold mb-8">
                  <GoldGradientText>Project Launch In</GoldGradientText>
                </h2>
                <CountdownTimer />
              </div>
              <div className="flex flex-col sm:flex-row gap-4 justify-center">
                <motion.div whileHover={{ scale: 1.05 }} whileTap={{ scale: 0.95 }}>
                  <Button className="bg-gradient-to-r from-[#FFD700] via-[#FDB931] to-[#BD9732] text-black hover:opacity-90 text-xl px-12 py-6">
                    Join Presale
                  </Button>
                </motion.div>
                <motion.div whileHover={{ scale: 1.05 }} whileTap={{ scale: 0.95 }}>
                  <Button variant="outline" className="border-2 border-[#BD9732] text-[#BD9732] hover:bg-[#BD9732]/10 text-xl px-12 py-6">
                    Learn More
                  </Button>
                </motion.div>
              </div>
            </motion.div>
          </motion.div>
        </section>

        <section id="about" className="py-32 relative">
          <div className="container mx-auto px-4">
            <motion.div
              initial={{ opacity: 0, y: 20 }}
              whileInView={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8 }}
              viewport={{ once: true }}
              className="text-center mb-16"
            >
              <h2 className="text-4xl md:text-5xl font-bold mb-6">
                <GoldGradientText>Why Million Dollar Egg?</GoldGradientText>
              </h2>
              <p className="text-xl text-[#BD9732] max-w-2xl mx-auto">
                A revolutionary digital asset that combines art, technology, and exclusivity
              </p>
            </motion.div>

            <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
              {features.map((feature, index) => (
                <motion.div
                  key={feature.title}
                  initial={{ opacity: 0, y: 20 }}
                  whileInView={{ opacity: 1, y: 0 }}
                  transition={{ duration: 0.5, delay: index * 0.1 }}
                  viewport={{ once: true }}
                >
                  <LuxuryCard>
                    <motion.div
                      className="text-[#FFD700] mb-4"
                      whileHover={{ scale: 1.1, rotate: 5 }}
                      transition={{ type: "spring", stiffness: 300 }}
                    >
                      {feature.icon}
                    </motion.div>
                    <h3 className="text-xl font-bold mb-2">
                      <GoldGradientText>{feature.title}</GoldGradientText>
                    </h3>
                    <p className="text-[#BD9732]">{feature.description}</p>
                  </LuxuryCard>
                </motion.div>
              ))}
            </div>
          </div>
        </section>

        <section id="nft" className="py-32 relative overflow-hidden">
          <motion.div
            className="absolute inset-0 opacity-10"
            animate={{
              backgroundImage: [
                "radial-gradient(circle, #FFD700 1px, transparent 1px)",
                "radial-gradient(circle, #BD9732 1px, transparent 1px)",
              ],
              backgroundSize: ["20px 20px", "30px 30px"],
            }}
            transition={{ duration: 10, repeat: Infinity, repeatType: "reverse" }}
          />
          <div className="container mx-auto px-4 relative z-10">
            <motion.div
              initial={{ opacity: 0, y: 20 }}
              whileInView={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8 }}
              viewport={{ once: true }}
              className="text-center mb-16"
            >
              <h2 className="text-4xl md:text-5xl font-bold mb-6">
                <GoldGradientText>NFT Collection</GoldGradientText>
              </h2>
              <p className="text-xl text-[#BD9732] max-w-2xl mx-auto">
                Exclusive digital eggs with unique properties and privileges
              </p>
            </motion.div>

            <div className="grid md:grid-cols-3 gap-8">
              {nftCollections.map((nft, index) => (
                <NFTCard key={nft.name} nft={nft} index={index} />
              ))}
            </div>
          </div>
        </section>

        <section id="vip" className="py-32 relative">
          <div className="container mx-auto px-4">
            <motion.div
              initial={{ opacity: 0, y: 20 }}
              whileInView={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8 }}
              viewport={{ once: true }}
              className="text-center mb-16"
            >
              <h2 className="text-4xl md:text-5xl font-bold mb-6">
                <GoldGradientText>VIP Membership</GoldGradientText>
              </h2>
              <p className="text-xl text-[#BD9732] max-w-2xl mx-auto">
                Unlock exclusive benefits and privileges with our VIP Golden Egg membership
              </p>
            </motion.div>
            <VIPGoldenEggCard />
          </div>
        </section>

        <section id="community" className="py-32 relative">
          <div className="container mx-auto px-4 text-center">
            <motion.div
              initial={{ opacity: 0, y: 20 }}
              whileInView={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8 }}
              viewport={{ once: true }}
              className="max-w-3xl mx-auto"
            >
              <h2 className="text-4xl md:text-5xl font-bold mb-6">
                <GoldGradientText>Join Our Community</GoldGradientText>
              </h2>
              <p className="text-xl text-[#BD9732] mb-8">
                Be part of an exclusive community of digital asset collectors and enthusiasts
              </p>
              <div className="flex flex-wrap justify-center gap-4">
                <motion.div whileHover={{ scale: 1.05 }} whileTap={{ scale: 0.95 }}>
                  <Button className="bg-gradient-to-r from-[#FFD700] via-[#FDB931] to-[#BD9732] text-black hover:opacity-90">
                    <MessageCircle className="w-5 h-5 mr-2" />
                    Discord
                  </Button>
                </motion.div>
                <motion.div whileHover={{ scale: 1.05 }} whileTap={{ scale: 0.95 }}>
                  <Button variant="outline" className="border-2 border-[#BD9732] text-[#BD9732] hover:bg-[#BD9732]/10">
                    <Twitter className="w-5 h-5 mr-2" />
                    Twitter
                  </Button>
                </motion.div>
                <motion.div whileHover={{ scale: 1.05 }} whileTap={{ scale: 0.95 }}>
                  <Button variant="outline" className="border-2 border-[#BD9732] text-[#BD9732] hover:bg-[#BD9732]/10">
                    <Send className="w-5 h-5 mr-2" />
                    Telegram
                  </Button>
                </motion.div>
              </div>
            </motion.div>
          </div>
        </section>
      </main>

      <footer className="border-t border-[#BD9732] py-8">
        <div className="container mx-auto px-4">
          <div className="flex flex-col md:flex-row justify-between items-center gap-4">
            <div className="text-[#BD9732]">
              © 2024 Million Dollar Egg. All rights reserved.
            </div>
            <div className="flex gap-4">
              <Link href="#" className="text-[#BD9732] hover:text-[#FFD700]">Terms</Link>
              <Link href="#" className="text-[#BD9732] hover:text-[#FFD700]">Privacy</Link>
            </div>
          </div>
        </div>
      </footer>

      <AnimatePresence>
        {scrollY.get() > 100 && (
          <motion.button
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: 20 }}
            className="fixed bottom-8 right-8 bg-gradient-to-r from-[#FFD700] via-[#FDB931] to-[#BD9732] text-black p-3 rounded-full shadow-lg"
            onClick={() => window.scrollTo({ top: 0, behavior: 'smooth' })}
          >
            <ChevronRight className="w-6 h-6 transform rotate-[-90deg]" />
          </motion.button>
        )}
      </AnimatePresence>
    </div>
  )
}