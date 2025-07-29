<template>
  <div>
    <v-row>
      <v-col cols="12" class="bg-secondary opacity-50">
        <div class="d-flex justify-space-between align-center">
          <div class="d-flex align-center">
            <v-icon class="mr-2">mdi-lock</v-icon>
            <h1 class="text-h5">Secrets Manager</h1>
          </div>
          <v-btn color="primary" @click="createSecretDialog = true">
            <v-icon class="mr-2">mdi-plus</v-icon>
            Criar Secret
          </v-btn>
        </div>
      </v-col>
    </v-row>

    <!-- Search and Filter -->
    <v-row>
      <v-col cols="12">
        <v-text-field
          v-model="searchQuery"
          label="Buscar secrets"
          prepend-inner-icon="mdi-magnify"
          variant="outlined"
          clearable
        >
          <template v-slot:append>
            <v-btn @click="loadSecrets" :loading="loading" variant="outlined">
              <v-icon class="mr-2">mdi-refresh</v-icon>
              Atualizar
            </v-btn>
          </template>
        </v-text-field>
      </v-col>
    </v-row>

    <!-- Secrets List -->
    <v-row>
      <v-col cols="12" md="6" lg="4" v-for="secret in filteredSecrets" :key="secret.ARN">
        <v-card class="mb-4" elevation="2">
          <v-card-title class="d-flex justify-space-between align-center">
            <TitleNameWithTooltip :name="secret.Name" />
            <v-menu>
              <template v-slot:activator="{ props }">
                <v-btn icon="mdi-dots-vertical" v-bind="props" size="small"></v-btn>
              </template>
              <v-list>
                <v-list-item @click="openSecret(secret)">
                  <v-list-item-title>
                    <v-icon class="mr-2">mdi-forum</v-icon>
                    Detalhes
                  </v-list-item-title>
                </v-list-item>
                <v-list-item @click="confirmDeleteSecret(secret)">
                  <v-list-item-title class="text-error">
                    <v-icon class="mr-2">mdi-delete</v-icon>
                    Excluir
                  </v-list-item-title>
                </v-list-item>
              </v-list>
            </v-menu>
          </v-card-title>
          <v-card-text>
            <v-chip size="small" class="mb-2">
              <v-icon start>mdi-text-box</v-icon>
              Descrição
            </v-chip>
            <p class="text-caption text-grey-darken-1">{{ secret.Description || "Sem descrição" }}</p>
          </v-card-text>
          <v-card-text>
            <v-chip size="small" class="mb-2">
              <v-icon start>mdi-identifier</v-icon>
              ARN
            </v-chip>
            <p class="text-caption text-break">{{ secret.ARN }}</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Empty State -->
    <v-row v-if="!loading && secrets.length === 0">
      <v-col cols="12" class="text-center">
        <v-icon size="120" color="grey-lighten-2">mdi-lock</v-icon>
        <h2 class="text-h5 text-grey-lighten-1 mt-4">Nenhum secret encontrado</h2>
        <p class="text-grey-lighten-1">Crie seu primeiro secret para começar</p>
        <v-btn color="primary" @click="createSecretDialog = true" class="mt-4">
          <v-icon class="mr-2">mdi-plus</v-icon>
          Criar Secret
        </v-btn>
      </v-col>
    </v-row>

    <!-- Loading State -->
    <v-row v-if="loading">
      <v-col cols="12" class="text-center">
        <v-progress-circular indeterminate size="64" color="primary"></v-progress-circular>
        <p class="mt-4 text-grey-lighten-1">Carregando secrets...</p>
      </v-col>
    </v-row>

    <!-- Create Secret Dialog -->
    <v-dialog v-model="createSecretDialog" max-width="500">
      <v-card>
        <v-card-title>
          <span class="text-h5">Criar Novo Secret</span>
        </v-card-title>
        <v-card-text>
          <v-text-field
            v-model="newSecretName"
            label="Nome do Secret"
            placeholder="my-secret"
            variant="outlined"
            :rules="[rules.required]"
            @keyup.enter="createSecret"
          ></v-text-field>
          <v-text-field
            v-model="newSecretDescription"
            label="Descrição do Secret"
            placeholder="my-secret-description"
            variant="outlined"
            @keyup.enter="createSecret"
          ></v-text-field>
          <v-textarea
            v-model="newSecretString"
            label="Conteudo do secret"
            placeholder='{&#10  "user": "user123",&#10  "password": "myPass123"&#10}'
            variant="outlined"
            rows="8"
            :rules="[rules.required]"
          ></v-textarea>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="createSecretDialog = false">Cancelar</v-btn>
          <v-btn
            color="primary"
            @click="createSecret"
            :loading="creating"
            :disabled="!newSecretName"
          >
            Criar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Secret Details Dialog -->
    <v-dialog v-model="secretDetailsDialog" max-width="800">
      <v-card v-if="selectedSecret">
        <v-card-title>
          <span class="text-h5">Detalhes do Secret</span>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="12">
              <v-card variant="outlined">
                <v-card-text>
                  <p><strong>Nome:</strong> {{ selectedSecret.Name }}</p>
                  <p><strong>Descrição:</strong> {{ selectedSecret.Description }}</p>
                  <p><strong>ARN:</strong> {{ selectedSecret.ARN }}</p>
                  <p><strong>Versões:</strong> {{ selectedSecret.Versions }}</p>

                  <p class="mt-4"><strong>Conteúdo do secret:</strong></p>
                  <textarea
                    cols="115"
                    rows="2"
                    readonly
                  >{{ selectedSecret.SecretString }}</textarea>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="secretDetailsDialog = false">Fechar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Delete Confirmation Dialog -->
    <v-dialog v-model="deleteDialog" max-width="400">
      <v-card>
        <v-card-title class="text-h5">Confirmar Exclusão</v-card-title>
        <v-card-text>
          Tem certeza que deseja excluir o secret <strong>{{ secretToDelete.Name }}</strong
          >?
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="deleteDialog = false">Cancelar</v-btn>
          <v-btn color="error" @click="deleteSecret" :loading="deleting">Excluir</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useAppStore } from '@/stores/app'
import { storeToRefs } from 'pinia'
import TitleNameWithTooltip from '@/components/TitleNameWithTooltip.vue'
import {
  ListSecretsCommand,
  CreateSecretCommand,
  GetSecretValueCommand,
  DescribeSecretCommand,
  DeleteSecretCommand,
} from '@aws-sdk/client-secrets-manager'

const appStore = useAppStore()
const { secretsManager } = storeToRefs(appStore)

// Reactive data
const secrets = ref([])
const searchQuery = ref('')
const loading = ref(false)
const creating = ref(false)
const deleting = ref(false)

// Dialog states
const createSecretDialog = ref(false)
const secretDetailsDialog = ref(false)
const deleteDialog = ref(false)

// Form data
const newSecretName = ref('')
const newSecretDescription = ref('')
const newSecretString = ref('')
const secretToDelete = ref(null)
const selectedSecret = ref(null)

// Validation rules
const rules = {
  required: value => !!value || 'Campo obrigatório',
}

// Computed properties
const filteredSecrets = computed(() => {
  if (!searchQuery.value) return secrets.value
  return secrets.value.filter(secret =>
    secret.Name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

// Secrets Manager API functions
const loadSecrets = async () => {
  try {
    loading.value = true
    const response = await secretsManager.value.send(new ListSecretsCommand({}))

    secrets.value = response.SecretList || []
  } catch (error) {
    console.error('Erro ao carregar secrets:', error)
    appStore.showSnackbar('Erro ao carregar os secrets', 'error')
  } finally {
    loading.value = false
  }
}

const createSecret = async () => {
  if (!newSecretName.value) return

  try {
    creating.value = true
    const response = await secretsManager.value.send(
      new CreateSecretCommand({
        Name: newSecretName.value,
        Description: newSecretDescription.value,
        SecretString: newSecretString.value,
      })
    )

    appStore.showSnackbar('Secret criado com sucesso!', 'success')
    createSecretDialog.value = false
    newSecretName.value = ''
    newSecretDescription.value = ''
    newSecretString.value = ''
    await loadSecrets()
  } catch (error) {
    console.error('Erro ao criar secret:', error)
    appStore.showSnackbar('Erro ao criar secret', 'error')
  } finally {
    creating.value = false
  }
}

const openSecret = async secret => {
  try {
    const described = await secretsManager.value.send(
     new DescribeSecretCommand({
       SecretId: secret.ARN,
     }))

    const secretContent = await secretsManager.value.send(
      new GetSecretValueCommand({
        SecretId: secret.ARN,
      })
    )

    selectedSecret.value = {
      Name: secret.Name,
      Description: secret.Description,
      ARN: secret.ARN,
      SecretString: secretContent.SecretString,
      Versions: described.VersionIdsToStages,
    }

    secretDetailsDialog.value = true
  } catch (error) {
    console.error('Erro ao carregar os detalhes do secret: ', error)
    appStore.showSnackbar('Erro ao carregar os detalhes do secret: ' + error, 'error')
  }
}

const confirmDeleteSecret = secret => {
  secretToDelete.value = { ARN: secret.ARN, Name: secret.Name }
  deleteDialog.value = true
}

const deleteSecret = async () => {
  try {
    deleting.value = true
    await secretsManager.value.send(
      new DeleteSecretCommand({
        SecretId: secretToDelete.value.Name,
        ForceDeleteWithoutRecovery: true,
      })
    )

    appStore.showSnackbar('Secret excluído com sucesso!', 'success')
    deleteDialog.value = false
    await loadSecrets()
  } catch (error) {
    console.error('Erro ao excluir secret:', error)
    appStore.showSnackbar('Erro ao excluir secret:' + error, 'error')
  } finally {
    deleting.value = false
  }
}

// Lifecycle
onMounted(() => {
  loadSecrets()
})
</script>

<style scoped>
.text-break {
  word-break: break-all;
}
</style>
