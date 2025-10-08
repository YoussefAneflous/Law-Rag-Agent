# Application d'Ingestion de Documents Juridiques Marocains

Cette application reproduit fidèlement la méthodologie de votre script Python Colab pour l'ingestion et l'analyse de documents juridiques marocains, avec une interface web moderne et sécurisée.

## 🚀 Fonctionnalités

### ✅ Reproduction Exacte du Script Python
- **Extraction de texte DOCX/DOC** : Paragraphes principaux + zones de texte (textboxes)
- **Normalisation arabe** : Suppression des diacritiques et normalisation des espaces
- **Extraction de métadonnées** : Utilisation du même prompt GPT-4o en arabe
- **Embeddings** : text-embedding-3-small d'OpenAI
- **Stockage vectoriel** : Pinecone avec métadonnées complètes

### 🌟 Améliorations Web
- **Interface bilingue** : Français/Arabe avec support RTL
- **Upload par lots** : Traitement multiple avec suivi de progression
- **Visualisation avancée** : Métadonnées structurées et recherche
- **Backend sécurisé** : Clés API protégées via Supabase Edge Functions

## 🏗️ Architecture

```
Frontend (React + TypeScript)
├── Extraction de texte (mammoth.js + JSZip + extraction binaire)
├── Interface bilingue (FR/AR)
└── Visualisation des métadonnées

Backend (Supabase Edge Functions)
├── process-legal-document
│   ├── Extraction métadonnées (GPT-4o)
│   ├── Création embeddings (text-embedding-3-small)
│   └── Stockage Pinecone
└── search-legal-documents
    ├── Embedding de requête
    └── Recherche vectorielle
```

## 📋 Métadonnées Extraites

Identiques au script Python :
- رقم_القرار (Numéro de décision)
- تاريخ_القرار (Date de décision)
- رقم_الملف_بالمحكمة_التجارية (Dossier tribunal commercial)
- رقم_الملف_بمحكمة_الاستئناف (Dossier cour d'appel)
- المحكمة (Tribunal)
- الهيئة_الحاكمة (Corps judiciaire)
- كاتب_الضبط (Greffier)
- المستأنف / المستأنف_عليه (Parties)
- المحامون (Avocats)
- Informations financières (montants, compensations)
- الحكم_النهائي (Décision finale)
- الأسباب_القانونية (Motifs juridiques)

## 🛠️ Configuration

### 1. Supabase Setup
1. Créer un projet Supabase
2. Créer le bucket de stockage :
   - Aller dans **Storage** dans le menu de gauche
   - Cliquer sur **+ New bucket**
   - Nom du bucket : `legal-documents`
   - Configurer l'accès public ou définir des politiques RLS appropriées
   - Cliquer sur **Create bucket**
3. Configurer les Edge Functions
4. Ajouter les variables d'environnement :
   - `OPENAI_API_KEY`
   - `PINECONE_API_KEY`

### 2. Pinecone Setup
1. Créer un index nommé `demo`
2. Dimension : 1536
3. Métrique : cosine
4. Cloud : AWS (us-east-1)

### 3. Configuration des clés API dans Supabase

**IMPORTANT**: Les clés API doivent être configurées dans Supabase Edge Functions, pas dans le fichier .env local.

1. Connectez-vous à votre [tableau de bord Supabase](https://supabase.com/dashboard)
2. Sélectionnez votre projet
3. Allez dans **Edge Functions** dans le menu de gauche
4. Pour chaque fonction (`process-legal-document`, `search-legal-documents`, `generate-rag-response`, `analyze-legal-decision`):
   - Cliquez sur la fonction
   - Allez dans l'onglet **Settings** ou **Environment Variables**
   - Ajoutez les variables suivantes :
     - `JINA_API_KEY`: Votre clé API Jina (OBLIGATOIRE pour les embeddings)
     - `CLAUDE_API_KEY`: Votre clé API Claude (recommandé)
     - `OPENAI_API_KEY`: Votre clé API OpenAI (fallback)
     - `PINECONE_API_KEY`: Votre clé API Pinecone
     - `PINECONE_INDEX_HOST`: L'URL complète de votre index Pinecone (ex: https://demo-xxxxxxx.svc.us-east-1.aws.pinecone.io)

### 4. Variables d'environnement locales
Copier `.env.example` vers `.env` et configurer :
```bash
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```

### 5. Vérification de la configuration

Pour vérifier que tout est correctement configuré :

1. **Pinecone** : Assurez-vous que l'index `demo` existe avec :
   - Dimension : 2048 (pour Jina V4)
   - Métrique : cosine
   - Région : us-east-1 (recommandé)

2. **OpenAI** : Vérifiez que votre clé API a accès aux modèles :
   - `claude-opus-4-20250514` (modèle principal recommandé)
   - `gpt-4o` (modèle de fallback)
   - `jina-embeddings-v4` (pour les embeddings - via API Jina)

3. **Claude** : Obtenez votre clé API sur [console.anthropic.com](https://console.anthropic.com)

4. **Jina** : Obtenez votre clé API sur [jina.ai](https://jina.ai) pour les embeddings V4

4. **Supabase Edge Functions** : Vérifiez que les variables d'environnement sont définies dans toutes les fonctions

## 🚀 Utilisation

1. **Upload** : Glissez-déposez vos fichiers DOCX/DOC/PDF/TXT
2. **Traitement** : Cliquez sur "Traiter tous les fichiers"
3. **Visualisation** : Consultez les métadonnées extraites
4. **Recherche** : Utilisez la recherche sémantique

## 🔒 Sécurité

- Clés API protégées côté serveur
- CORS configuré
- Validation des entrées
- Gestion d'erreurs robuste

## 📱 Interface

- **Responsive** : Mobile et desktop
- **Bilingue** : Français/Arabe avec RTL
- **Accessible** : Standards WCAG
- **Moderne** : Design professionnel

Cette application offre la même puissance que votre script Python avec l'avantage d'une interface utilisateur moderne et sécurisée !