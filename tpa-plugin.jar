```java
package com.deinname.simpletpa;

import org.bukkit.Bukkit;
import org.bukkit.ChatColor;
import org.bukkit.Location;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.player.PlayerCommandPreprocessEvent;
import org.bukkit.event.player.PlayerQuitEvent;
import org.bukkit.plugin.java.JavaPlugin;
import org.bukkit.scheduler.BukkitRunnable;

import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
/**
 *@author Raphael Troppmann & Max
 *@version 2024-04-07
 *@einfaches tpa plugin, mit tutorials gemacht

public class SimpleTPA extends JavaPlugin implements Listener {

    private Map<UUID, UUID> pendingRequests = new HashMap<>(); // Map für ausstehende TPA-Anfragen

    @Override
    public void onEnable() {
        getLogger().info("SimpleTPA Plugin aktiviert.");
        Bukkit.getPluginManager().registerEvents(this, this);
    }

    @Override
    public void onDisable() {
        getLogger().info("SimpleTPA Plugin deaktiviert.");
    }

    @EventHandler
    public void onPlayerQuit(PlayerQuitEvent event) {
        Player player = event.getPlayer();
        // Entferne Spielerdaten beim Ausloggen
        pendingRequests.remove(player.getUniqueId());
    }

    @EventHandler
    public void onPlayerCommandPreprocess(PlayerCommandPreprocessEvent event) {
        Player player = event.getPlayer();
        String[] args = event.getMessage().split(" ");
        String command = args[0].toLowerCase();

        // /tpa [player] - Teleportationsanfrage senden
        if (command.equals("/tpa") && args.length == 2) {
            Player target = Bukkit.getPlayer(args[1]);
            if (target != null && target.isOnline()) {
                sendeAnfrage(player, target);
                event.setCancelled(true);
            } else {
                player.sendMessage(ChatColor.RED + "Spieler nicht gefunden oder offline.");
            }
        }
        // /tpaccept - Anfrage akzeptieren
        else if (command.equals("/tpaccept") && args.length == 1) {
            anfrageAkzeptieren(player);
            event.setCancelled(true);
        }
    }

    // Methode zur Sendung einer Teleportationsanfrage
    private void sendeAnfrage(Player sender, Player empfaenger) {
        pendingRequests.put(empfaenger.getUniqueId(), sender.getUniqueId());
        empfaenger.sendMessage(ChatColor.YELLOW + "Teleportationsanfrage von " + sender.getName() +
                " erhalten. Mit /tpaccept annehmen oder mit /tpdeny ablehnen.");
        sender.sendMessage(ChatColor.YELLOW + "Teleportationsanfrage an " + empfaenger.getName() + " gesendet.");
    }

    // Methode zur Akzeptierung einer Teleportationsanfrage
    private void anfrageAkzeptieren(Player empfaenger) {
        UUID senderUUID = pendingRequests.get(empfaenger.getUniqueId());
        if (senderUUID != null) {
            Player sender = Bukkit.getPlayer(senderUUID);
            if (sender != null && sender.isOnline()) {
                sender.sendMessage(ChatColor.GREEN + empfaenger.getName() + " hat deine Teleportationsanfrage akzeptiert.");
                empfaenger.sendMessage(ChatColor.GREEN + "Teleportiere...");
                teleportiereNach3Sekunden(sender, empfaenger.getLocation());
            } else {
                empfaenger.sendMessage(ChatColor.RED + "Der Absender ist nicht mehr online.");
            }
            pendingRequests.remove(empfaenger.getUniqueId());
        } else {
            empfaenger.sendMessage(ChatColor.RED + "Du hast keine ausstehenden Teleportationsanfragen.");
        }
    }

    // Methode zum automatischen Teleportieren nach 3 Sekunden
    private void teleportiereNach3Sekunden(Player player, Location targetLocation) {
        new BukkitRunnable() {
            @Override
            public void run() {
                player.teleport(targetLocation);
            }
        }.runTaskLater(this, 60); // 60 ticks entsprechen 3 Sekunden (20 ticks pro Sekunde)
    }
}
