'use client'
import { useState } from 'react'
import Link from 'next/link'
import { useRouter } from 'next/navigation'
import { Phone, Eye, EyeOff } from 'lucide-react'

const COUNTRIES = [
  { code: 'EG', name: 'مصر', flag: '🇪🇬' },
  { code: 'SA', name: 'السعودية', flag: '🇸🇦' },
  { code: 'AE', name: 'الإمارات', flag: '🇦🇪' },
  { code: 'KW', name: 'الكويت', flag: '🇰🇼' },
  { code: 'JO', name: 'الأردن', flag: '🇯🇴' },
  { code: 'MA', name: 'المغرب', flag: '🇲🇦' },
  { code: 'US', name: 'USA', flag: '🇺🇸' },
  { code: 'GB', name: 'UK', flag: '🇬🇧' },
  { code: 'FR', name: 'France', flag: '🇫🇷' },
  { code: 'DE', name: 'Germany', flag: '🇩🇪' },
  { code: 'OTHER', name: 'أخرى', flag: '🌍' },
]

const LANGUAGES = ['العربية', 'English', 'Français', 'Español', 'Deutsch', 'Türkçe']

const AVATAR_COLORS = ['#1D9E75', '#7F77DD', '#D85A30', '#D4537E', '#BA7517', '#185FA5', '#639922']

export default function RegisterPage() {
  const router = useRouter()
  const [step, setStep] = useState(1)
  const [loading, setLoading] = useState(false)
  const [showPass, setShowPass] = useState(false)
  const [error, setError] = useState('')

  const [form, setForm] = useState({
    firstName: '', lastName: '', email: '', password: '',
    country: 'EG', languages: ['العربية'], bio: '',
    avatarColor: AVATAR_COLORS[0],
  })

  function set(k: string, v: any) {
    setForm(p => ({ ...p, [k]: v }))
  }

  function toggleLang(lang: string) {
    set('languages', form.languages.includes(lang)
      ? form.languages.filter(l => l !== lang)
      : [...form.languages, lang])
  }

  async function handleRegister() {
    setLoading(true)
    setError('')
    try {
      const res = await fetch('/api/auth/register', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(form),
      })
      const data = await res.json()
      if (!res.ok) { setError(data.error || 'حدث خطأ'); setLoading(false); return }

      localStorage.setItem('ihear_token', data.token)
      localStorage.setItem('ihear_user', JSON.stringify(data.user))
      router.push('/subscribe')
    } catch {
      setError('حدث خطأ في الاتصال')
      setLoading(false)
    }
  }

  const stepDots = [1, 2, 3]

  return (
    <div className="min-h-screen bg-dark-1 flex items-center justify-center px-4 py-12">
      <div className="w-full max-w-md">
        {/* Logo */}
        <Link href="/" className="flex items-center gap-2 justify-center mb-8">
          <div className="w-9 h-9 bg-teal-mid rounded-xl flex items-center justify-center">
            <Phone size={18} className="text-white" />
          </div>
          <span className="text-2xl font-black">I <span className="text-teal-mid">Hear</span></span>
        </Link>

        <div className="card p-8">
          {/* Step indicator */}
          <div className="flex gap-2 mb-6">
            {stepDots.map(s => (
              <div key={s} className={`flex-1 h-1 rounded-full transition-colors ${s <= step ? 'bg-teal-mid' : 'bg-white/10'}`} />
            ))}
          </div>

          {/* STEP 1 */}
          {step === 1 && (
            <div className="animate-fade-in">
              <h2 className="text-2xl font-black mb-1">إنشاء حساب جديد</h2>
              <p className="text-gray-400 text-sm mb-6">خطوة 1 من 3 — معلوماتك الأساسية</p>

              <div className="grid grid-cols-2 gap-3 mb-3">
                <div>
                  <label className="block text-xs text-gray-400 mb-1.5">الاسم الأول</label>
                  <input className="input-field" placeholder="محمد" value={form.firstName}
                    onChange={e => set('firstName', e.target.value)} />
                </div>
                <div>
                  <label className="block text-xs text-gray-400 mb-1.5">اسم العائلة</label>
                  <input className="input-field" placeholder="أحمد" value={form.lastName}
                    onChange={e => set('lastName', e.target.value)} />
                </div>
              </div>

              <div className="mb-3">
                <label className="block text-xs text-gray-400 mb-1.5">الإيميل</label>
                <input className="input-field" type="email" placeholder="email@example.com"
                  value={form.email} onChange={e => set('email', e.target.value)} />
              </div>

              <div className="mb-5">
                <label className="block text-xs text-gray-400 mb-1.5">كلمة السر</label>
                <div className="relative">
                  <input className="input-field pl-10" type={showPass ? 'text' : 'password'}
                    placeholder="٨ أحرف على الأقل" value={form.password}
                    onChange={e => set('password', e.target.value)} />
                  <button onClick={() => setShowPass(!showPass)}
                    className="absolute left-3 top-1/2 -translate-y-1/2 text-gray-500 hover:text-gray-300">
                    {showPass ? <EyeOff size={16} /> : <Eye size={16} />}
                  </button>
                </div>
              </div>

              {error && <p className="text-red-400 text-sm mb-4">{error}</p>}

              <button className="btn-primary w-full py-3"
                onClick={() => {
                  if (!form.firstName || !form.email || form.password.length < 8) {
                    setError('تأكد من ملء كل الحقول وكلمة السر ٨ أحرف على الأقل')
                    return
                  }
                  setError('')
                  setStep(2)
                }}>
                التالي ←
              </button>
            </div>
          )}

          {/* STEP 2 */}
          {step === 2 && (
            <div className="animate-fade-in">
              <h2 className="text-2xl font-black mb-1">بروفايلك الشخصي</h2>
              <p className="text-gray-400 text-sm mb-6">خطوة 2 من 3 — عشان الناس تعرفك</p>

              <div className="mb-3">
                <label className="block text-xs text-gray-400 mb-1.5">دولتك</label>
                <select className="input-field" value={form.country} onChange={e => set('country', e.target.value)}>
                  {COUNTRIES.map(c => (
                    <option key={c.code} value={c.code}>{c.flag} {c.name}</option>
                  ))}
                </select>
              </div>

              <div className="mb-3">
                <label className="block text-xs text-gray-400 mb-1.5">اللغات</label>
                <div className="flex flex-wrap gap-2">
                  {LANGUAGES.map(lang => (
                    <button key={lang} onClick={() => toggleLang(lang)}
                      className={`px-3 py-1.5 rounded-lg text-xs border transition-all ${
                        form.languages.includes(lang)
                          ? 'bg-teal-mid/20 border-teal-mid text-teal-mid'
                          : 'border-white/10 text-gray-400 hover:border-white/20'
                      }`}>
                      {lang}
                    </button>
                  ))}
                </div>
              </div>

              <div className="mb-3">
                <label className="block text-xs text-gray-400 mb-1.5">نبذة عنك (Bio)</label>
                <textarea className="input-field resize-none" rows={3}
                  placeholder="أخبر الناس عن نفسك وخبراتك..."
                  value={form.bio} onChange={e => set('bio', e.target.value)} />
              </div>

              <div className="mb-5">
                <label className="block text-xs text-gray-400 mb-1.5">لون أفاتارك</label>
                <div className="flex gap-2">
                  {AVATAR_COLORS.map(c => (
                    <button key={c} onClick={() => set('avatarColor', c)}
                      className={`w-7 h-7 rounded-full transition-all ${form.avatarColor === c ? 'ring-2 ring-white ring-offset-1 ring-offset-dark-1 scale-110' : ''}`}
                      style={{ background: c }} />
                  ))}
                </div>
              </div>

              <div className="flex gap-3">
                <button className="btn-outline flex-1 py-3" onClick={() => setStep(1)}>→ رجوع</button>
                <button className="btn-primary flex-1 py-3" onClick={() => setStep(3)}>التالي ←</button>
              </div>
            </div>
          )}

          {/* STEP 3 */}
          {step === 3 && (
            <div className="animate-fade-in">
              <h2 className="text-2xl font-black mb-1">اختر طريقة الدفع</h2>
              <p className="text-gray-400 text-sm mb-5">خطوة 3 من 3 — اشتراك شهري $2 فقط</p>

              <div className="bg-teal-mid/10 border border-teal-mid/30 rounded-xl p-4 mb-5 text-sm">
                ✅ سيتم خصم <strong className="text-white">$2.00</strong> شهرياً عبر Stripe الآمن
                <br />
                <span className="text-gray-400 text-xs">يمكنك الإلغاء في أي وقت</span>
              </div>

              <div className="grid grid-cols-3 gap-2 mb-5">
                {[
                  { icon: '💳', label: 'Visa / MC' },
                  { icon: '🅿️', label: 'PayPal' },
                  { icon: '💠', label: 'Stripe' },
                  { icon: '📱', label: 'فودافون كاش' },
                  { icon: '🟠', label: 'أورنج كاش' },
                  { icon: '💚', label: 'إتصالات كاش' },
                  { icon: '🍎', label: 'Apple Pay' },
                  { icon: '🟡', label: 'Google Pay' },
                  { icon: '🏦', label: 'تحويل بنكي' },
                ].map(p => (
                  <div key={p.label}
                    className="border border-white/[0.08] rounded-xl p-3 text-center text-xs
                               hover:border-teal-mid/50 hover:bg-teal-mid/5 cursor-pointer transition-all">
                    <div className="text-2xl mb-1">{p.icon}</div>
                    {p.label}
                  </div>
                ))}
              </div>

              {error && <p className="text-red-400 text-sm mb-4">{error}</p>}

              <div className="flex gap-3">
                <button className="btn-outline py-3 px-4" onClick={() => setStep(2)}>→</button>
                <button className="btn-primary flex-1 py-3 font-bold" onClick={handleRegister} disabled={loading}>
                  {loading ? '⏳ جاري التسجيل...' : '🚀 سجّل وادفع الآن'}
                </button>
              </div>
            </div>
          )}

          <p className="text-center text-gray-500 text-xs mt-5">
            عندك حساب؟{' '}
            <Link href="/login" className="text-teal-mid hover:underline">سجّل دخول</Link>
          </p>
        </div>
      </div>
    </div>
  )
}
