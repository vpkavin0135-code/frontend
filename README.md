# FULL SATCK DEVELOPMENT

import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Login',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        fontFamily: 'Poppins',
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF7C3AED),
          brightness: Brightness.dark,
        ),
        useMaterial3: true,
      ),
      home: const LoginPage(),
    );
  }
}

// ─────────────────────────────────────────────
//  LOGIN PAGE
// ─────────────────────────────────────────────
class LoginPage extends StatefulWidget {
  const LoginPage({super.key});

  @override
  State<LoginPage> createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage>
    with SingleTickerProviderStateMixin {
  final _formKey = GlobalKey<FormState>();
  final _emailCtrl = TextEditingController();
  final _passCtrl = TextEditingController();

  bool _obscurePassword = true;
  bool _rememberMe = false;
  bool _isLoading = false;

  late AnimationController _animController;
  late Animation<double> _fadeAnim;
  late Animation<Offset> _slideAnim;

  // ── Purple palette ──
  static const Color _bg = Color(0xFF0F0015);
  static const Color _p2 = Color(0xFF6D28D9);
  static const Color _p3 = Color(0xFFA855F7);
  static const Color _p4 = Color(0xFFD8B4FE);
  static const Color _accent = Color(0xFFF0ABFC);
  static const Color _surface = Color(0x0DFFFFFF);
  static const Color _borderColor = Color(0x30A855F7);

  @override
  void initState() {
    super.initState();
    _animController = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 900),
    );
    _fadeAnim = CurvedAnimation(
      parent: _animController,
      curve: const Interval(0.0, 0.7, curve: Curves.easeOut),
    );
    _slideAnim = Tween<Offset>(
      begin: const Offset(0, 0.08),
      end: Offset.zero,
    ).animate(CurvedAnimation(
      parent: _animController,
      curve: const Interval(0.1, 0.9, curve: Curves.easeOutCubic),
    ));
    _animController.forward();
  }

  @override
  void dispose() {
    _animController.dispose();
    _emailCtrl.dispose();
    _passCtrl.dispose();
    super.dispose();
  }

  Future<void> _handleLogin() async {
    if (!_formKey.currentState!.validate()) return;
    setState(() => _isLoading = true);
    await Future.delayed(const Duration(seconds: 2));
    setState(() => _isLoading = false);
    if (!mounted) return;
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: const Text('✦  Welcome back!'),
        backgroundColor: _p2,
        behavior: SnackBarBehavior.floating,
        shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: _bg,
      body: Stack(
        children: [
          // Background gradient orbs
          _buildBackground(),

          // Main content
          SafeArea(
            child: Center(
              child: SingleChildScrollView(
                padding: const EdgeInsets.symmetric(horizontal: 28, vertical: 24),
                child: FadeTransition(
                  opacity: _fadeAnim,
                  child: SlideTransition(
                    position: _slideAnim,
                    child: _buildCard(),
                  ),
                ),
              ),
            ),
          ),
        ],
      ),
    );
  }

  // ── Background blobs ──
  Widget _buildBackground() {
    return Stack(
      children: [
        Positioned(
          top: -150,
          left: -120,
          child: _orb(480, const Color(0xFF5B21B6), 0.35),
        ),
        Positioned(
          bottom: -100,
          right: -100,
          child: _orb(360, const Color(0xFF7C3AED), 0.30),
        ),
        Positioned(
          top: MediaQuery.of(context).size.height * 0.4,
          right: MediaQuery.of(context).size.width * 0.2,
          child: _orb(220, const Color(0xFFC026D3), 0.22),
        ),
      ],
    );
  }

  Widget _orb(double size, Color color, double opacity) {
    return Container(
      width: size,
      height: size,
      decoration: BoxDecoration(
        shape: BoxShape.circle,
        color: color.withOpacity(opacity),
        boxShadow: [
          BoxShadow(
            color: color.withOpacity(opacity * 0.6),
            blurRadius: 120,
            spreadRadius: 40,
          ),
        ],
      ),
    );
  }

  // ── Glass card ──
  Widget _buildCard() {
    return Container(
      width: double.infinity,
      constraints: const BoxConstraints(maxWidth: 400),
      decoration: BoxDecoration(
        color: Colors.white.withOpacity(0.04),
        borderRadius: BorderRadius.circular(32),
        border: Border.all(color: _borderColor, width: 1),
        boxShadow: [
          BoxShadow(
            color: Colors.black.withOpacity(0.5),
            blurRadius: 60,
            offset: const Offset(0, 24),
          ),
        ],
      ),
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          // Top accent line
          Container(
            height: 5,
            decoration: const BoxDecoration(
              borderRadius: BorderRadius.vertical(top: Radius.circular(32)),
              gradient: LinearGradient(
                colors: [_p2, _p3, _accent],
              ),
            ),
          ),

          Padding(
            padding: const EdgeInsets.fromLTRB(28, 32, 28, 32),
            child: Form(
              key: _formKey,
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  _buildHeader(),
                  const SizedBox(height: 32),
                  _buildEmailField(),
                  const SizedBox(height: 16),
                  _buildPasswordField(),
                  const SizedBox(height: 14),
                  _buildRememberForgotRow(),
                  const SizedBox(height: 24),
                  _buildLoginButton(),
                  const SizedBox(height: 22),
                  _buildDivider(),
                  const SizedBox(height: 18),
                  _buildSocialButtons(),
                  const SizedBox(height: 20),
                  _buildSignUpRow(),
                ],
              ),
            ),
          ),

          // Bottom home indicator bar
          Container(
            height: 40,
            alignment: Alignment.center,
            child: Container(
              width: 80,
              height: 4,
              decoration: BoxDecoration(
                color: Colors.white.withOpacity(0.12),
                borderRadius: BorderRadius.circular(2),
              ),
            ),
          ),
        ],
      ),
    );
  }

  // ── Header ──
  Widget _buildHeader() {
    return Column(
      children: [
        // Logo
        Container(
          width: 68,
          height: 68,
          decoration: BoxDecoration(
            borderRadius: BorderRadius.circular(20),
            gradient: const LinearGradient(
              begin: Alignment.topLeft,
              end: Alignment.bottomRight,
              colors: [_p2, _p3],
            ),
            boxShadow: [
              BoxShadow(
                color: _p2.withOpacity(0.5),
                blurRadius: 24,
                offset: const Offset(0, 10),
              ),
            ],
          ),
          child: const Center(
            child: Text('✦', style: TextStyle(fontSize: 28, color: Colors.white)),
          ),
        ),
        const SizedBox(height: 18),
        const Text(
          'Welcome back',
          style: TextStyle(
            color: Colors.white,
            fontSize: 26,
            fontWeight: FontWeight.w800,
            letterSpacing: -0.5,
          ),
        ),
        const SizedBox(height: 6),
        Text(
          'Sign in to continue your journey',
          style: TextStyle(
            color: Colors.white.withOpacity(0.4),
            fontSize: 13,
            fontWeight: FontWeight.w300,
          ),
        ),
      ],
    );
  }

  // ── Email field ──
  Widget _buildEmailField() {
    return _buildField(
      controller: _emailCtrl,
      label: 'EMAIL ADDRESS',
      hint: 'hello@example.com',
      icon: Icons.mail_outline_rounded,
      keyboardType: TextInputType.emailAddress,
      validator: (v) {
        if (v == null || v.isEmpty) return 'Email is required';
        if (!v.contains('@')) return 'Enter a valid email';
        return null;
      },
    );
  }

  // ── Password field ──
  Widget _buildPasswordField() {
    return _buildField(
      controller: _passCtrl,
      label: 'PASSWORD',
      hint: 'Enter your password',
      icon: Icons.lock_outline_rounded,
      obscureText: _obscurePassword,
      suffixIcon: IconButton(
        icon: Icon(
          _obscurePassword ? Icons.visibility_outlined : Icons.visibility_off_outlined,
          color: Colors.white.withOpacity(0.3),
          size: 18,
        ),
        onPressed: () => setState(() => _obscurePassword = !_obscurePassword),
      ),
      validator: (v) {
        if (v == null || v.isEmpty) return 'Password is required';
        if (v.length < 6) return 'At least 6 characters';
        return null;
      },
    );
  }

  // ── Reusable field builder ──
  Widget _buildField({
    required TextEditingController controller,
    required String label,
    required String hint,
    required IconData icon,
    TextInputType keyboardType = TextInputType.text,
    bool obscureText = false,
    Widget? suffixIcon,
    String? Function(String?)? validator,
  }) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          label,
          style: TextStyle(
            color: Colors.white.withOpacity(0.45),
            fontSize: 10.5,
            fontWeight: FontWeight.w600,
            letterSpacing: 1.2,
          ),
        ),
        const SizedBox(height: 7),
        TextFormField(
          controller: controller,
          keyboardType: keyboardType,
          obscureText: obscureText,
          validator: validator,
          style: const TextStyle(color: Colors.white, fontSize: 14),
          decoration: InputDecoration(
            hintText: hint,
            hintStyle: TextStyle(color: Colors.white.withOpacity(0.2), fontSize: 14),
            prefixIcon: Icon(icon, color: _p3, size: 18),
            suffixIcon: suffixIcon,
            filled: true,
            fillColor: Colors.white.withOpacity(0.05),
            enabledBorder: OutlineInputBorder(
              borderRadius: BorderRadius.circular(14),
              borderSide: const BorderSide(color: _borderColor),
            ),
            focusedBorder: OutlineInputBorder(
              borderRadius: BorderRadius.circular(14),
              borderSide: const BorderSide(color: _p3, width: 1.5),
            ),
            errorBorder: OutlineInputBorder(
              borderRadius: BorderRadius.circular(14),
              borderSide: const BorderSide(color: Color(0xFFF87171)),
            ),
            focusedErrorBorder: OutlineInputBorder(
              borderRadius: BorderRadius.circular(14),
              borderSide: const BorderSide(color: Color(0xFFF87171), width: 1.5),
            ),
            errorStyle: const TextStyle(color: Color(0xFFF87171), fontSize: 11),
            contentPadding: const EdgeInsets.symmetric(horizontal: 16, vertical: 14),
          ),
        ),
      ],
    );
  }

  // ── Remember me + Forgot password ──
  Widget _buildRememberForgotRow() {
    return Row(
      children: [
        GestureDetector(
          onTap: () => setState(() => _rememberMe = !_rememberMe),
          child: Row(
            children: [
              AnimatedContainer(
                duration: const Duration(milliseconds: 200),
                width: 18,
                height: 18,
                decoration: BoxDecoration(
                  borderRadius: BorderRadius.circular(5),
                  color: _rememberMe ? _p3 : Colors.transparent,
                  border: Border.all(
                    color: _rememberMe ? _p3 : _borderColor,
                    width: 1.5,
                  ),
                ),
                child: _rememberMe
                    ? const Icon(Icons.check, color: Colors.white, size: 12)
                    : null,
              ),
              const SizedBox(width: 8),
              Text(
                'Remember me',
                style: TextStyle(
                  color: Colors.white.withOpacity(0.45),
                  fontSize: 12.5,
                ),
              ),
            ],
          ),
        ),
        const Spacer(),
        GestureDetector(
          onTap: () {},
          child: const Text(
            'Forgot password?',
            style: TextStyle(
              color: _p4,
              fontSize: 12.5,
              fontWeight: FontWeight.w500,
            ),
          ),
        ),
      ],
    );
  }

  // ── Login Button ──
  Widget _buildLoginButton() {
    return SizedBox(
      height: 52,
      child: DecoratedBox(
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(16),
          gradient: const LinearGradient(
            colors: [_p2, _p3],
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
          ),
          boxShadow: [
            BoxShadow(
              color: _p2.withOpacity(0.45),
              blurRadius: 20,
              offset: const Offset(0, 8),
            ),
          ],
        ),
        child: ElevatedButton(
          onPressed: _isLoading ? null : _handleLogin,
          style: ElevatedButton.styleFrom(
            backgroundColor: Colors.transparent,
            shadowColor: Colors.transparent,
            foregroundColor: Colors.white,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(16),
            ),
          ),
          child: _isLoading
              ? const SizedBox(
                  width: 20,
                  height: 20,
                  child: CircularProgressIndicator(
                    color: Colors.white,
                    strokeWidth: 2,
                  ),
                )
              : const Text(
                  'Sign In',
                  style: TextStyle(
                    fontSize: 16,
                    fontWeight: FontWeight.w700,
                    letterSpacing: 0.3,
                  ),
                ),
        ),
      ),
    );
  }

  // ── Divider ──
  Widget _buildDivider() {
    return Row(
      children: [
        Expanded(
          child: Divider(color: Colors.white.withOpacity(0.08), thickness: 1),
        ),
        Padding(
          padding: const EdgeInsets.symmetric(horizontal: 14),
          child: Text(
            'or continue with',
            style: TextStyle(
              color: Colors.white.withOpacity(0.2),
              fontSize: 11.5,
            ),
          ),
        ),
        Expanded(
          child: Divider(color: Colors.white.withOpacity(0.08), thickness: 1),
        ),
      ],
    );
  }

  // ── Social Buttons ──
  Widget _buildSocialButtons() {
    return Row(
      children: [
        Expanded(child: _socialBtn('G', 'Google', const Color(0xFF4285F4))),
        const SizedBox(width: 12),
        Expanded(child: _socialBtn('⌘', 'Apple', Colors.white)),
      ],
    );
  }

  Widget _socialBtn(String icon, String label, Color iconColor) {
    return OutlinedButton(
      onPressed: () {},
      style: OutlinedButton.styleFrom(
        padding: const EdgeInsets.symmetric(vertical: 12),
        side: BorderSide(color: Colors.white.withOpacity(0.08)),
        backgroundColor: Colors.white.withOpacity(0.04),
        foregroundColor: Colors.white.withOpacity(0.7),
        shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(14)),
      ),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Text(
            icon,
            style: TextStyle(
              color: iconColor,
              fontSize: 15,
              fontWeight: FontWeight.w700,
            ),
          ),
          const SizedBox(width: 8),
          Text(
            label,
            style: const TextStyle(fontSize: 13, fontWeight: FontWeight.w500),
          ),
        ],
      ),
    );
  }

  // ── Sign Up Row ──
  Widget _buildSignUpRow() {
    return Center(
      child: RichText(
        text: TextSpan(
          style: TextStyle(
            color: Colors.white.withOpacity(0.35),
            fontSize: 13,
          ),
          children: [
            const TextSpan(text: "Don't have an account? "),
            WidgetSpan(
              child: GestureDetector(
                onTap: () {},
                child: const Text(
                  'Create one →',
                  style: TextStyle(
                    color: _p4,
                    fontSize: 13,
                    fontWeight: FontWeight.w600,
                  ),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
