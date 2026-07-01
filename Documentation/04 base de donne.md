# 04 - Base de Données

## Vue d'ensemble
Base de données PostgreSQL 16 pour Sethios.me. Conception normalisée avec support JSON, full-text search natif et partitionnement pour les tables à forte croissance.

## Tables principales

### users
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- name : VARCHAR(255) NOT NULL
- email : VARCHAR(255) NOT NULL UNIQUE
- email_verified_at : TIMESTAMP NULL
- password : VARCHAR(255) NOT NULL
- avatar : VARCHAR(255) NULL
- bio : TEXT NULL
- title : VARCHAR(255) NULL
- company : VARCHAR(255) NULL
- website : VARCHAR(255) NULL
- github : VARCHAR(255) NULL
- linkedin : VARCHAR(255) NULL
- twitter : VARCHAR(255) NULL
- google_id : VARCHAR(255) NULL UNIQUE
- github_id : VARCHAR(255) NULL UNIQUE
- linkedin_id : VARCHAR(255) NULL UNIQUE
- two_factor_secret : TEXT NULL
- two_factor_recovery_codes : TEXT NULL
- two_factor_confirmed_at : TIMESTAMP NULL
- locale : VARCHAR(5) DEFAULT 'fr'
- last_login_at : TIMESTAMP NULL
- last_login_ip : VARCHAR(45) NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL (soft deletes)

Index : email, uuid, google_id, github_id, linkedin_id, last_login_at

### roles
- id : BIGSERIAL PRIMARY KEY
- name : VARCHAR(255) NOT NULL UNIQUE
- guard_name : VARCHAR(255) NOT NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

### permissions
- id : BIGSERIAL PRIMARY KEY
- name : VARCHAR(255) NOT NULL UNIQUE
- guard_name : VARCHAR(255) NOT NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Tables pivot : model_has_roles, model_has_permissions, role_has_permissions (Spatie)

### categories
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- name : VARCHAR(255) NOT NULL
- slug : VARCHAR(255) NOT NULL UNIQUE
- description : TEXT NULL
- type : VARCHAR(50) NOT NULL (article, project, publication, book, ai_model)
- parent_id : BIGINT NULL REFERENCES categories(id) ON DELETE SET NULL
- sort_order : INTEGER DEFAULT 0
- is_active : BOOLEAN DEFAULT true
- meta_title : VARCHAR(255) NULL
- meta_description : TEXT NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : slug, type, parent_id, (type, slug) UNIQUE

### tags
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- name : JSONB NOT NULL (ex: {"fr": "Intelligence Artificielle", "en": "Artificial Intelligence"})
- slug : JSONB NOT NULL (ex: {"fr": "intelligence-artificielle", "en": "artificial-intelligence"})
- type : VARCHAR(50) NULL
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : GIN sur name et slug pour recherche multilingue

Table pivot : taggables (Spatie Tags)
- tag_id : BIGINT NOT NULL REFERENCES tags(id) ON DELETE CASCADE
- taggable_type : VARCHAR(255) NOT NULL
- taggable_id : BIGINT NOT NULL
- created_at : TIMESTAMP NOT NULL

Index : (taggable_type, taggable_id), tag_id

### projects
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- title : VARCHAR(255) NOT NULL
- slug : VARCHAR(255) NOT NULL UNIQUE
- description : TEXT NOT NULL
- content : TEXT NOT NULL (Markdown)
- excerpt : VARCHAR(500) NULL
- type : VARCHAR(50) NOT NULL (personal, professional, open_source, collaborative)
- status : VARCHAR(50) NOT NULL (draft, in_progress, completed, maintained, archived)
- difficulty_level : INTEGER DEFAULT 1 (1-5)
- impact : TEXT NULL
- results : TEXT NULL
- client_name : VARCHAR(255) NULL
- client_testimonial : TEXT NULL
- github_url : VARCHAR(255) NULL
- demo_url : VARCHAR(255) NULL
- start_date : DATE NULL
- end_date : DATE NULL
- completion_percentage : INTEGER DEFAULT 0
- sort_order : INTEGER DEFAULT 0
- is_featured : BOOLEAN DEFAULT false
- is_published : BOOLEAN DEFAULT false
- published_at : TIMESTAMP NULL
- meta_title : VARCHAR(255) NULL
- meta_description : TEXT NULL
- views_count : BIGINT DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : slug, status, type, is_featured, published_at, difficulty_level
Full-text : GIN sur to_tsvector('french', title || ' ' || description || ' ' || content)

Table pivot : category_project (project_id, category_id)
Table pivot : project_technology
- project_id : BIGINT NOT NULL REFERENCES projects(id) ON DELETE CASCADE
- technology_id : BIGINT NOT NULL REFERENCES technologies(id) ON DELETE CASCADE
- sort_order : INTEGER DEFAULT 0

### technologies
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- name : VARCHAR(255) NOT NULL UNIQUE
- slug : VARCHAR(255) NOT NULL UNIQUE
- icon : VARCHAR(255) NULL (class FontAwesome ou SVG)
- color : VARCHAR(7) NULL (code hex)
- category : VARCHAR(50) NULL (language, framework, database, tool, cloud, other)
- website : VARCHAR(255) NULL
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : slug, category

### articles
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- title : VARCHAR(255) NOT NULL
- slug : VARCHAR(255) NOT NULL UNIQUE
- excerpt : TEXT NULL
- content : TEXT NOT NULL (Markdown)
- reading_time : INTEGER DEFAULT 0 (minutes)
- status : VARCHAR(50) NOT NULL (draft, review, ready, published, scheduled, archived)
- is_featured : BOOLEAN DEFAULT false
- is_published : BOOLEAN DEFAULT false
- published_at : TIMESTAMP NULL
- scheduled_at : TIMESTAMP NULL
- author_id : BIGINT NOT NULL REFERENCES users(id)
- meta_title : VARCHAR(255) NULL
- meta_description : TEXT NULL
- og_image : VARCHAR(255) NULL
- canonical_url : VARCHAR(255) NULL
- views_count : BIGINT DEFAULT 0
- unique_views_count : BIGINT DEFAULT 0
- comments_count : INTEGER DEFAULT 0
- shares_count : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : slug, status, is_featured, published_at, author_id, reading_time
Full-text : GIN sur to_tsvector('french', title || ' ' || excerpt || ' ' || content)

Table pivot : article_category (article_id, category_id)

### comments
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- commentable_type : VARCHAR(255) NOT NULL (Article, Project, Book)
- commentable_id : BIGINT NOT NULL
- user_id : BIGINT NULL REFERENCES users(id) ON DELETE SET NULL
- parent_id : BIGINT NULL REFERENCES comments(id) ON DELETE CASCADE
- author_name : VARCHAR(255) NULL (si non connecté)
- author_email : VARCHAR(255) NULL
- author_website : VARCHAR(255) NULL
- content : TEXT NOT NULL
- status : VARCHAR(50) NOT NULL DEFAULT 'pending' (pending, approved, spam, rejected)
- ip_address : VARCHAR(45) NULL
- user_agent : TEXT NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : (commentable_type, commentable_id), parent_id, status, created_at

### publications
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- title : VARCHAR(500) NOT NULL
- slug : VARCHAR(500) NOT NULL UNIQUE
- authors : JSONB NOT NULL (ex: [{"name": "Sethios", "affiliation": "Independent"}])
- abstract : TEXT NOT NULL
- content : TEXT NULL
- type : VARCHAR(50) NOT NULL (journal_article, conference_paper, book_chapter, thesis, preprint, report)
- journal_name : VARCHAR(255) NULL
- conference_name : VARCHAR(255) NULL
- volume : VARCHAR(50) NULL
- issue : VARCHAR(50) NULL
- pages : VARCHAR(50) NULL
- publisher : VARCHAR(255) NULL
- doi : VARCHAR(255) NULL UNIQUE
- isbn : VARCHAR(50) NULL
- issn : VARCHAR(50) NULL
- publication_date : DATE NOT NULL
- year : INTEGER GENERATED ALWAYS AS (EXTRACT(YEAR FROM publication_date)) STORED
- language : VARCHAR(50) DEFAULT 'fr'
- keywords : JSONB NULL
- citations_count : INTEGER DEFAULT 0
- pdf_path : VARCHAR(255) NULL
- url : VARCHAR(500) NULL
- bibtex : TEXT NULL
- status : VARCHAR(50) DEFAULT 'published' (draft, submitted, accepted, published)
- is_featured : BOOLEAN DEFAULT false
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : slug, year, type, publication_date, status, is_featured
Full-text : GIN sur to_tsvector('french', title || ' ' || abstract)

Table pivot : publication_category (publication_id, category_id)

### books
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- title : VARCHAR(500) NOT NULL
- slug : VARCHAR(500) NOT NULL UNIQUE
- author : VARCHAR(255) NOT NULL
- description : TEXT NULL
- isbn : VARCHAR(50) NULL UNIQUE
- publisher : VARCHAR(255) NULL
- publication_year : INTEGER NULL
- edition : VARCHAR(100) NULL
- pages_count : INTEGER NULL
- language : VARCHAR(50) DEFAULT 'fr'
- file_path : VARCHAR(255) NULL
- file_size : BIGINT NULL
- file_type : VARCHAR(50) NULL
- cover_image : VARCHAR(255) NULL
- downloads_count : INTEGER DEFAULT 0
- views_count : BIGINT DEFAULT 0
- is_public : BOOLEAN DEFAULT true
- is_downloadable : BOOLEAN DEFAULT true
- is_featured : BOOLEAN DEFAULT false
- meta_title : VARCHAR(255) NULL
- meta_description : TEXT NULL
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : slug, author, publication_year, is_public, is_featured
Full-text : GIN sur to_tsvector('french', title || ' ' || author || ' ' || description)

Table pivot : book_category (book_id, category_id)

### skills
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- name : VARCHAR(255) NOT NULL UNIQUE
- slug : VARCHAR(255) NOT NULL UNIQUE
- type : VARCHAR(50) NOT NULL (technical, soft, language, tool)
- category : VARCHAR(100) NULL
- icon : VARCHAR(255) NULL
- color : VARCHAR(7) NULL
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : slug, type, category

Table pivot : skill_user
- skill_id : BIGINT NOT NULL REFERENCES skills(id) ON DELETE CASCADE
- user_id : BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE
- level : INTEGER DEFAULT 1 (1-5)
- years_experience : INTEGER DEFAULT 0
- is_highlighted : BOOLEAN DEFAULT false
- sort_order : INTEGER DEFAULT 0

### experiences
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- user_id : BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE
- company : VARCHAR(255) NOT NULL
- position : VARCHAR(255) NOT NULL
- description : TEXT NULL
- location : VARCHAR(255) NULL
- type : VARCHAR(50) DEFAULT 'full_time' (full_time, part_time, freelance, internship, volunteer)
- is_current : BOOLEAN DEFAULT false
- start_date : DATE NOT NULL
- end_date : DATE NULL
- company_website : VARCHAR(255) NULL
- company_logo : VARCHAR(255) NULL
- achievements : JSONB NULL (ex: ["Increased performance by 40%", "Led team of 5 developers"])
- technologies_used : JSONB NULL
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : user_id, (start_date, end_date), is_current

### education
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- user_id : BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE
- institution : VARCHAR(255) NOT NULL
- degree : VARCHAR(255) NOT NULL
- field_of_study : VARCHAR(255) NOT NULL
- description : TEXT NULL
- location : VARCHAR(255) NULL
- start_date : DATE NOT NULL
- end_date : DATE NULL
- is_current : BOOLEAN DEFAULT false
- gpa : VARCHAR(10) NULL
- honors : JSONB NULL
- activities : JSONB NULL
- institution_logo : VARCHAR(255) NULL
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : user_id, (start_date, end_date)

### certifications
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- user_id : BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE
- name : VARCHAR(255) NOT NULL
- issuing_organization : VARCHAR(255) NOT NULL
- credential_id : VARCHAR(255) NULL
- credential_url : VARCHAR(500) NULL
- issue_date : DATE NOT NULL
- expiration_date : DATE NULL
- does_not_expire : BOOLEAN DEFAULT false
- description : TEXT NULL
- badge_image : VARCHAR(255) NULL
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : user_id, issue_date

### contracts
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- reference : VARCHAR(50) NOT NULL UNIQUE (ex: CON-2026-001)
- client_id : BIGINT NOT NULL REFERENCES users(id)
- title : VARCHAR(255) NOT NULL
- description : TEXT NOT NULL
- type : VARCHAR(50) NOT NULL (freelance, consulting, training, research, other)
- status : VARCHAR(50) NOT NULL (draft, sent, signed, in_progress, completed, cancelled, archived)
- amount : DECIMAL(10,2) NULL
- currency : VARCHAR(3) DEFAULT 'EUR'
- billing_type : VARCHAR(50) NULL (fixed, hourly, daily, monthly)
- hourly_rate : DECIMAL(8,2) NULL
- start_date : DATE NULL
- end_date : DATE NULL
- signed_at : TIMESTAMP NULL
- completed_at : TIMESTAMP NULL
- contract_template : VARCHAR(255) NULL
- contract_file_path : VARCHAR(255) NULL
- signed_file_path : VARCHAR(255) NULL
- terms_and_conditions : TEXT NULL
- deliverables : JSONB NULL
- milestones : JSONB NULL (ex: [{"title": "Phase 1", "deadline": "2026-08-01", "amount": 5000, "completed": true}])
- payment_terms : VARCHAR(255) NULL
- payment_status : VARCHAR(50) DEFAULT 'pending' (pending, partial, paid, overdue)
- notes : TEXT NULL
- is_public : BOOLEAN DEFAULT false
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : reference UNIQUE, client_id, status, type, (start_date, end_date), payment_status

### contract_messages
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- contract_id : BIGINT NOT NULL REFERENCES contracts(id) ON DELETE CASCADE
- user_id : BIGINT NOT NULL REFERENCES users(id)
- message : TEXT NOT NULL
- attachments : JSONB NULL
- is_read : BOOLEAN DEFAULT false
- created_at : TIMESTAMP NOT NULL

Index : contract_id, user_id, created_at

### contract_activities
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- contract_id : BIGINT NOT NULL REFERENCES contracts(id) ON DELETE CASCADE
- user_id : BIGINT NOT NULL REFERENCES users(id)
- action : VARCHAR(255) NOT NULL (created, sent, signed, updated_status, added_milestone, completed_milestone, added_message, uploaded_file)
- description : TEXT NULL
- metadata : JSONB NULL
- created_at : TIMESTAMP NOT NULL

Index : contract_id, created_at

### opportunities
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- type : VARCHAR(50) NOT NULL (employment, freelance, consulting, collaboration, conference, research, other)
- title : VARCHAR(255) NOT NULL
- company_name : VARCHAR(255) NOT NULL
- company_website : VARCHAR(255) NULL
- company_logo : VARCHAR(255) NULL
- contact_name : VARCHAR(255) NULL
- contact_email : VARCHAR(255) NULL
- contact_phone : VARCHAR(50) NULL
- description : TEXT NOT NULL
- requirements : TEXT NULL
- budget_range : VARCHAR(100) NULL
- currency : VARCHAR(3) DEFAULT 'EUR'
- location : VARCHAR(255) NULL
- is_remote : BOOLEAN DEFAULT false
- deadline : DATE NULL
- status : VARCHAR(50) NOT NULL (received, reviewed, in_discussion, accepted, rejected, archived)
- priority : INTEGER DEFAULT 1 (1-3)
- source : VARCHAR(100) NULL (email, linkedin, website, referral, conference)
- expected_duration : VARCHAR(100) NULL
- notes : TEXT NULL
- attachments : JSONB NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : status, type, priority, deadline, created_at

### ai_projects
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- name : VARCHAR(255) NOT NULL
- slug : VARCHAR(255) NOT NULL UNIQUE
- description : TEXT NOT NULL
- content : TEXT NULL (documentation Markdown)
- type : VARCHAR(50) NOT NULL (nlp, computer_vision, generative, predictive, recommendation, other)
- status : VARCHAR(50) NOT NULL (research, development, testing, deployed, archived)
- framework : VARCHAR(100) NULL (TensorFlow, PyTorch, JAX, Hugging Face, custom)
- language : VARCHAR(50) NULL (Python, R, Julia)
- accuracy : DECIMAL(5,2) NULL
- f1_score : DECIMAL(5,2) NULL
- other_metrics : JSONB NULL
- training_time : INTERVAL NULL
- dataset_size : BIGINT NULL
- dataset_description : TEXT NULL
- dataset_url : VARCHAR(500) NULL
- model_path : VARCHAR(255) NULL
- notebook_path : VARCHAR(255) NULL
- github_url : VARCHAR(255) NULL
- demo_url : VARCHAR(255) NULL
- api_endpoint : VARCHAR(255) NULL
- is_public : BOOLEAN DEFAULT true
- is_api_available : BOOLEAN DEFAULT false
- requires_auth : BOOLEAN DEFAULT false
- rate_limit : INTEGER DEFAULT 60
- parameters_count : BIGINT NULL
- model_size_mb : DECIMAL(10,2) NULL
- inference_time_ms : DECIMAL(10,2) NULL
- paper_url : VARCHAR(500) NULL
- blog_article_id : BIGINT NULL REFERENCES articles(id) ON DELETE SET NULL
- is_featured : BOOLEAN DEFAULT false
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : slug, type, status, is_public, is_featured
Full-text : GIN sur to_tsvector('english', name || ' ' || description)

Table pivot : ai_project_tag (ai_project_id, tag_id)
Table pivot : ai_project_category (ai_project_id, category_id)

### ai_model_versions
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- ai_project_id : BIGINT NOT NULL REFERENCES ai_projects(id) ON DELETE CASCADE
- version : VARCHAR(50) NOT NULL (ex: v1.0.0)
- description : TEXT NULL
- accuracy : DECIMAL(5,2) NULL
- metrics : JSONB NULL
- changelog : TEXT NULL
- model_file_path : VARCHAR(255) NOT NULL
- config_file_path : VARCHAR(255) NULL
- weights_file_path : VARCHAR(255) NULL
- is_active : BOOLEAN DEFAULT false
- training_date : TIMESTAMP NULL
- training_duration : INTERVAL NULL
- training_dataset_version : VARCHAR(50) NULL
- created_at : TIMESTAMP NOT NULL

Index : (ai_project_id, version) UNIQUE, is_active

### ai_predictions
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- ai_project_id : BIGINT NOT NULL REFERENCES ai_projects(id) ON DELETE CASCADE
- ai_model_version_id : BIGINT NOT NULL REFERENCES ai_model_versions(id)
- user_id : BIGINT NULL REFERENCES users(id)
- input_data : JSONB NOT NULL
- output_data : JSONB NOT NULL
- confidence_score : DECIMAL(5,2) NULL
- processing_time_ms : DECIMAL(10,2) NULL
- ip_address : VARCHAR(45) NULL
- user_agent : TEXT NULL
- is_correct : BOOLEAN NULL (feedback utilisateur)
- feedback : TEXT NULL
- created_at : TIMESTAMP NOT NULL

Index : ai_project_id, user_id, created_at
Partition : PAR RANGE (created_at) (mensuelle)

### media
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- model_type : VARCHAR(255) NOT NULL
- model_id : BIGINT NOT NULL
- collection_name : VARCHAR(255) NOT NULL
- name : VARCHAR(255) NOT NULL
- file_name : VARCHAR(255) NOT NULL
- mime_type : VARCHAR(255) NULL
- disk : VARCHAR(255) NOT NULL
- conversions_disk : VARCHAR(255) NULL
- size : BIGINT NOT NULL
- manipulations : JSONB NOT NULL
- custom_properties : JSONB NOT NULL
- generated_conversions : JSONB NOT NULL
- responsive_images : JSONB NOT NULL
- sort_order : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : (model_type, model_id, collection_name), uuid
Table Spatie Media Library standard

### subscribers
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- email : VARCHAR(255) NOT NULL UNIQUE
- name : VARCHAR(255) NULL
- locale : VARCHAR(5) DEFAULT 'fr'
- is_verified : BOOLEAN DEFAULT false
- verified_at : TIMESTAMP NULL
- verification_token : VARCHAR(100) NULL
- unsubscribe_token : VARCHAR(100) NOT NULL UNIQUE
- subscribed_at : TIMESTAMP NOT NULL
- unsubscribed_at : TIMESTAMP NULL
- status : VARCHAR(50) DEFAULT 'active' (active, unsubscribed, bounced)
- source : VARCHAR(100) NULL (website, blog, portfolio, download)
- preferences : JSONB NULL (ex: {"new_articles": true, "new_projects": false, "newsletter": true})
- last_email_sent_at : TIMESTAMP NULL
- emails_received_count : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : email UNIQUE, status, subscribed_at, unsubscribe_token UNIQUE

### newsletter_campaigns
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- subject : VARCHAR(255) NOT NULL
- content : TEXT NOT NULL
- preview_text : VARCHAR(255) NULL
- status : VARCHAR(50) NOT NULL (draft, scheduled, sending, sent, failed)
- scheduled_at : TIMESTAMP NULL
- sent_at : TIMESTAMP NULL
- recipients_count : INTEGER DEFAULT 0
- opened_count : INTEGER DEFAULT 0
- clicked_count : INTEGER DEFAULT 0
- bounced_count : INTEGER DEFAULT 0
- unsubscribed_count : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : status, scheduled_at, sent_at

### messages
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- name : VARCHAR(255) NOT NULL
- email : VARCHAR(255) NOT NULL
- subject : VARCHAR(255) NOT NULL
- message : TEXT NOT NULL
- type : VARCHAR(50) DEFAULT 'contact' (contact, freelance, collaboration, bug_report, other)
- status : VARCHAR(50) DEFAULT 'unread' (unread, read, replied, archived, spam)
- priority : INTEGER DEFAULT 1
- ip_address : VARCHAR(45) NULL
- user_agent : TEXT NULL
- attachments : JSONB NULL
- replied_at : TIMESTAMP NULL
- reply_content : TEXT NULL
- conversation_id : BIGINT NULL REFERENCES messages(id)
- auto_detected_subject : VARCHAR(255) NULL
- sentiment_score : DECIMAL(3,2) NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : status, email, type, created_at, conversation_id

### visitors
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- session_id : VARCHAR(255) NOT NULL
- ip_address : VARCHAR(45) NULL
- user_agent : TEXT NULL
- device : VARCHAR(50) NULL
- browser : VARCHAR(100) NULL
- operating_system : VARCHAR(100) NULL
- country : VARCHAR(100) NULL
- city : VARCHAR(100) NULL
- language : VARCHAR(10) NULL
- referrer : TEXT NULL
- utm_source : VARCHAR(255) NULL
- utm_medium : VARCHAR(255) NULL
- utm_campaign : VARCHAR(255) NULL
- is_new_visitor : BOOLEAN DEFAULT true
- first_visit_at : TIMESTAMP NOT NULL
- last_visit_at : TIMESTAMP NOT NULL
- visits_count : INTEGER DEFAULT 1
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : session_id, ip_address, (first_visit_at, last_visit_at)
Partition : PAR RANGE (first_visit_at) (mensuelle)

### page_views
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- visitor_id : BIGINT NOT NULL REFERENCES visitors(id) ON DELETE CASCADE
- page_url : TEXT NOT NULL
- page_title : VARCHAR(255) NULL
- route_name : VARCHAR(255) NULL
- viewable_type : VARCHAR(255) NULL
- viewable_id : BIGINT NULL
- time_spent_seconds : INTEGER DEFAULT 0
- is_bounce : BOOLEAN DEFAULT true
- exit_page : BOOLEAN DEFAULT false
- scroll_percentage : INTEGER DEFAULT 0
- created_at : TIMESTAMP NOT NULL

Index : visitor_id, (viewable_type, viewable_id), created_at
Partition : PAR RANGE (created_at) (mensuelle)

### testimonials
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- client_name : VARCHAR(255) NOT NULL
- client_position : VARCHAR(255) NULL
- client_company : VARCHAR(255) NOT NULL
- client_avatar : VARCHAR(255) NULL
- content : TEXT NOT NULL
- rating : INTEGER NULL CHECK (rating BETWEEN 1 AND 5)
- project_id : BIGINT NULL REFERENCES projects(id) ON DELETE SET NULL
- contract_id : BIGINT NULL REFERENCES contracts(id) ON DELETE SET NULL
- is_verified : BOOLEAN DEFAULT false
- is_featured : BOOLEAN DEFAULT false
- language : VARCHAR(5) DEFAULT 'fr'
- sort_order : INTEGER DEFAULT 0
- displayed_at : TIMESTAMP NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL
- deleted_at : TIMESTAMP NULL

Index : is_featured, project_id, is_verified

### seo_metadata
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- seoable_type : VARCHAR(255) NOT NULL
- seoable_id : BIGINT NOT NULL
- meta_title : VARCHAR(255) NULL
- meta_description : TEXT NULL
- meta_keywords : JSONB NULL
- og_title : VARCHAR(255) NULL
- og_description : TEXT NULL
- og_image : VARCHAR(255) NULL
- og_type : VARCHAR(50) NULL
- twitter_card : VARCHAR(50) DEFAULT 'summary_large_image'
- twitter_title : VARCHAR(255) NULL
- twitter_description : TEXT NULL
- twitter_image : VARCHAR(255) NULL
- canonical_url : VARCHAR(500) NULL
- robots : VARCHAR(100) DEFAULT 'index,follow'
- schema_markup : JSONB NULL
- structured_data : JSONB NULL
- focus_keyword : VARCHAR(255) NULL
- readability_score : DECIMAL(3,1) NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : (seoable_type, seoable_id) UNIQUE, focus_keyword

### search_queries
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- query : VARCHAR(500) NOT NULL
- results_count : INTEGER DEFAULT 0
- searchable_type : VARCHAR(255) NULL
- user_id : BIGINT NULL REFERENCES users(id) ON DELETE SET NULL
- visitor_id : BIGINT NULL REFERENCES visitors(id) ON DELETE SET NULL
- ip_address : VARCHAR(45) NULL
- locale : VARCHAR(5) DEFAULT 'fr'
- created_at : TIMESTAMP NOT NULL

Index : query, searchable_type, created_at
Partition : PAR RANGE (created_at) (mensuelle)

### downloads
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- downloadable_type : VARCHAR(255) NOT NULL (Book, Publication, Project)
- downloadable_id : BIGINT NOT NULL
- user_id : BIGINT NULL REFERENCES users(id) ON DELETE SET NULL
- visitor_id : BIGINT NULL REFERENCES visitors(id) ON DELETE SET NULL
- ip_address : VARCHAR(45) NULL
- file_name : VARCHAR(255) NOT NULL
- file_size : BIGINT NULL
- created_at : TIMESTAMP NOT NULL

Index : (downloadable_type, downloadable_id), user_id, created_at

### backup_logs
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- backup_name : VARCHAR(255) NOT NULL
- disk : VARCHAR(255) NOT NULL
- file_path : VARCHAR(500) NULL
- size : BIGINT NULL
- type : VARCHAR(50) NOT NULL (full, database, files)
- status : VARCHAR(50) NOT NULL (success, failed, in_progress)
- error_message : TEXT NULL
- started_at : TIMESTAMP NOT NULL
- completed_at : TIMESTAMP NULL
- created_at : TIMESTAMP NOT NULL

Index : backup_name, status, started_at

### activity_log
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- log_name : VARCHAR(255) NULL
- description : TEXT NOT NULL
- subject_type : VARCHAR(255) NULL
- subject_id : BIGINT NULL
- causer_type : VARCHAR(255) NULL
- causer_id : BIGINT NULL
- properties : JSONB NULL
- event : VARCHAR(255) NULL
- batch_uuid : UUID NULL
- ip_address : VARCHAR(45) NULL
- created_at : TIMESTAMP NOT NULL

Index : log_name, (subject_type, subject_id), (causer_type, causer_id), created_at
Table Spatie Activity Log standard

### notifications
- id : UUID PRIMARY KEY
- type : VARCHAR(255) NOT NULL
- notifiable_type : VARCHAR(255) NOT NULL
- notifiable_id : BIGINT NOT NULL
- data : TEXT NOT NULL
- read_at : TIMESTAMP NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : (notifiable_type, notifiable_id), read_at
Table Laravel Notifications standard

### personal_access_tokens
- id : BIGSERIAL PRIMARY KEY
- tokenable_type : VARCHAR(255) NOT NULL
- tokenable_id : BIGINT NOT NULL
- name : VARCHAR(255) NOT NULL
- token : VARCHAR(64) NOT NULL UNIQUE
- abilities : TEXT NULL
- last_used_at : TIMESTAMP NULL
- expires_at : TIMESTAMP NULL
- created_at : TIMESTAMP NOT NULL
- updated_at : TIMESTAMP NOT NULL

Index : token UNIQUE, (tokenable_type, tokenable_id)
Table Laravel Sanctum standard

### sessions
- id : VARCHAR(255) PRIMARY KEY
- user_id : BIGINT NULL
- ip_address : VARCHAR(45) NULL
- user_agent : TEXT NULL
- payload : TEXT NOT NULL
- last_activity : INTEGER NOT NULL

Index : user_id, last_activity
Table Laravel Sessions standard

### cache
- key : VARCHAR(255) PRIMARY KEY
- value : TEXT NOT NULL
- expiration : INTEGER NOT NULL

Index : expiration

### cache_locks
- key : VARCHAR(255) PRIMARY KEY
- owner : VARCHAR(255) NOT NULL
- expiration : INTEGER NOT NULL

### jobs
- id : BIGSERIAL PRIMARY KEY
- queue : VARCHAR(255) NOT NULL
- payload : TEXT NOT NULL
- attempts : INTEGER NOT NULL
- reserved_at : INTEGER NULL
- available_at : INTEGER NOT NULL
- created_at : INTEGER NOT NULL

Index : queue

### job_batches
- id : VARCHAR(255) PRIMARY KEY
- name : VARCHAR(255) NOT NULL
- total_jobs : INTEGER NOT NULL
- pending_jobs : INTEGER NOT NULL
- failed_jobs : INTEGER NOT NULL
- failed_job_ids : TEXT NULL
- options : TEXT NULL
- cancelled_at : INTEGER NULL
- finished_at : INTEGER NULL
- created_at : INTEGER NOT NULL

### failed_jobs
- id : BIGSERIAL PRIMARY KEY
- uuid : UUID NOT NULL UNIQUE
- connection : TEXT NOT NULL
- queue : TEXT NOT NULL
- payload : TEXT NOT NULL
- exception : TEXT NOT NULL
- failed_at : TIMESTAMP NOT NULL

## Indexation full-text search

### Configuration PostgreSQL
```sql
-- Configuration recherche française
CREATE TEXT SEARCH CONFIGURATION fr (COPY = french);
ALTER TEXT SEARCH CONFIGURATION fr ALTER MAPPING FOR hword, hword_part, word WITH unaccent, french_stem;

-- Index pour articles
CREATE INDEX idx_articles_search ON articles USING GIN (
    to_tsvector('fr', coalesce(title, '') || ' ' || coalesce(excerpt, '') || ' ' || coalesce(content, ''))
);

-- Index pour projects
CREATE INDEX idx_projects_search ON projects USING GIN (
    to_tsvector('fr', coalesce(title, '') || ' ' || coalesce(description, '') || ' ' || coalesce(content, ''))
);

-- Index pour publications
CREATE INDEX idx_publications_search ON publications USING GIN (
    to_tsvector('fr', coalesce(title, '') || ' ' || coalesce(abstract, ''))
);

-- Index pour books
CREATE INDEX idx_books_search ON books USING GIN (
    to_tsvector('fr', coalesce(title, '') || ' ' || coalesce(author, '') || ' ' || coalesce(description, ''))
);

-- Index pour ai_projects
CREATE INDEX idx_ai_projects_search ON ai_projects USING GIN (
    to_tsvector('english', coalesce(name, '') || ' ' || coalesce(description, ''))
);