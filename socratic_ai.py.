import random

class SocraticAI:
    def __init__(self):
        # State management
        self.consent_status = {}  # Tracks user consent for high-stakes topics
        self.awaiting_consent_for = None  # Tracks if the system is waiting for consent

        # Pillar 1: Epistemic Humility Phrases
        self.epistemic_humility_phrases = [
            "My understanding is limited, but one perspective is that...",
            "This is a complex topic, and I may not have the full picture, but...",
            "Based on the data I have, it seems that...",
            "It's important to consider other viewpoints, but my analysis suggests...",
        ]

        # Pillar 2: Friction-to-Trust Socratic Questions
        self.socratic_questions = [
            "What are the underlying assumptions you're making with that query?",
            "Could you elaborate on what you mean by that?",
            "What is the core problem you are trying to solve?",
            "Are there alternative perspectives to consider here?",
            "How would you define success for this query?",
        ]

        # Pillar 3: Tiered Consent Gateway Keywords
        self.high_stakes_topics = {
            "medical": ["symptom", "diagnosis", "treatment", "medical advice"],
            "financial": ["invest", "stock", "market", "financial plan", "retire"],
            "legal": ["lawsuit", "legal advice", "contract", "sue"],
        }

    def _check_for_high_stakes(self, user_input: str) -> str | None:
        """Identifies if the input relates to a high-stakes topic."""
        for topic, keywords in self.high_stakes_topics.items():
            if any(keyword in user_input for keyword in keywords):
                return topic
        return None

    def _should_introduce_friction(self, user_input: str) -> bool:
        """Decides whether to ask a Socratic question."""
        # For demonstration, friction is introduced with a 30% probability
        # A more advanced system would analyze query complexity.
        return random.random() < 0.3

    def _call_llm_api(self, user_input: str) -> str:
        """Placeholder for the actual LLM API call."""
        return f"LLM analysis of '{user_input}'"

    def process_input(self, user_input: str) -> str:
        """Processes user input according to the Socratic Mandate."""
        normalized_input = user_input.lower()

        # --- Pillar 3: Tiered Consent Gateway ---
        # Check if we are currently waiting for consent
        if self.awaiting_consent_for:
            if "yes" in normalized_input or "i agree" in normalized_input:
                topic = self.awaiting_consent_for
                self.consent_status[topic] = True
                self.awaiting_consent_for = None
                return f"Consent acknowledged for {topic}. You may now proceed with your query."
            else:
                topic = self.awaiting_consent_for
                self.awaiting_consent_for = None
                return f"Consent not provided for {topic}. I cannot proceed with this high-stakes query."

        # Check for new high-stakes queries
        high_stakes_topic = self._check_for_high_stakes(normalized_input)
        if high_stakes_topic and not self.consent_status.get(high_stakes_topic):
            self.awaiting_consent_for = high_stakes_topic
            return (f"WARNING: Your query touches on the high-stakes topic of '{high_stakes_topic}'. "
                    "AI-generated information in this domain can be incomplete or inaccurate. "
                    "Please confirm you understand the risks and wish to proceed by responding with 'yes'.")

        # --- Pillar 2: Friction-to-Trust Protocol ---
        if self._should_introduce_friction(normalized_input):
            return random.choice(self.socratic_questions)

        # --- Pillar 1: Epistemic Humility & LLM Call ---
        llm_response = self._call_llm_api(user_input)
        humility_phrase = random.choice(self.epistemic_humility_phrases)

        return f"{humility_phrase} {llm_response}"

# --- Example Usage ---
ai = SocraticAI()

# 1. Standard Query (may trigger friction or a humble response)
print(f"User: What is the capital of France?")
print(f"AI: {ai.process_input('What is the capital of France?')}\n")

# 2. High-Stakes Query (triggers consent gateway)
print(f"User: Should I invest in the stock market?")
print(f"AI: {ai.process_input('Should I invest in the stock market?')}\n")

# 3. User gives consent
print(f"User: yes, I agree")
print(f"AI: {ai.process_input('yes, I agree')}\n")

# 4. User re-asks the high-stakes query (now with consent)
print(f"User: Should I invest in the stock market?")
print(f"AI: {ai.process_input('Should I invest in the stock market?')}\n")

# 5. Another high-stakes query in a different domain
print(f"User: What are the legal implications of this contract?")
print(f"AI: {ai.process_input('What are the legal implications of this contract?')}\n")
