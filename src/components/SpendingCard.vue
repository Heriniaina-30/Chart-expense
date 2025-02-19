
<script setup>
    import { ref, onMounted, watch } from "vue";
    import { Chart, registerables } from "chart.js";
    
    Chart.register(...registerables);
    
    const spendingData = ref([]);
    const chartCanvas = ref(null);
    let chartInstance = null;
    
    const balance = ref(921.48); // Solde affiché
    const totalSpending = ref(0); // Total dépenses
    
    // Charger les données JSON
    const fetchData = async () => {
      try {
        const response = await fetch("/data.json"); 
        spendingData.value = await response.json();
    
        // Calculer le total
        totalSpending.value = spendingData.value.reduce((sum, item) => sum + item.amount, 0);
      } catch (error) {
        console.error(error);
      }
    };
    
    // Dessiner le graphique
    const renderChart = (data) => {
      if (!chartCanvas.value) return;
    
      if (chartInstance) chartInstance.destroy(); // Éviter les doublons
    
      const maxAmount = Math.max(...data);
      
      chartInstance = new Chart(chartCanvas.value, {
        type: "bar",
        data: {
          labels: spendingData.value.map((item) => item.day),
          datasets: [{
            data: data,
            backgroundColor: data.map((amount) => amount === maxAmount ? "#76B5BC" : "#EC755D"),
            borderRadius: 3,
          }],
        },
        options: {
          responsive: true,
          plugins: { legend: { display: false } },
          scales: { y: { display: false }, x: { grid: { display: false } } },
        }
      });
    };
    
    // Mettre à jour le graphique quand les données changent
    watch(spendingData, (newData) => {
      if (newData.length > 0) {
        renderChart(newData.map(item => item.amount));
      }
    });
    
    onMounted(fetchData);
    </script>
    
    <template>
      <div class="container">
        <!-- Balance -->
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="balance-card">
                    <div>
                      <h1 class="fs-6 text-start fw-light">My balance</h1>
                      <h2>${{ balance.toFixed(2) }}</h2>
                    </div>
                    <div class="toggle-switch"><img src="../assets/images/logo.svg" alt=""></div>
                  </div>
              
                  <!-- Spending Chart -->
                  <div class="chart-card mt-3">
                    <h3>Spending – Last 7 days</h3>
                    <canvas ref="chartCanvas"></canvas>
                    <hr />
                    <div class="summary pt-3">
                      <div class="text-start">
                        <p class="fw-light text-muted">Total this month</p>
                        <h2 class="total fs-1">${{ totalSpending.toFixed(2) }}</h2>
                      </div>
                      <div class="text-end">
                        <h2 class="fs-6">+2.4%</h2>
                        <p class="percentage fw-light"> from last month</p>
                      </div>
                    </div>
                  </div>
                </div>
            </div>
        </div>        
    </template>
    
    <style scoped>
    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 20px;
      background-color: #f8e9dd;
      min-height: 100vh;
      padding-top: 50px;
    }
    .balance-card, .chart-card {
        width: 100% !important;
        max-width: 1000px; /* Ou une largeur définie */
        margin: auto; /* Centre l'élément */
      }
    
      .total{
        margin-top: -1.2rem;
      }


    /* Balance card */
    .balance-card {
      background-color: #ec755d;
      color: white;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px 20px;
      border-radius: 10px;
      width: 300px;
    }
    
    
    /* Chart card */
    .chart-card {
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      width: 300px;
      box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
    }
    
    .summary {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    
    .percentage {
      color: #a05c4f;
    }
    </style>
    